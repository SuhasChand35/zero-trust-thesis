# Lab build guide: Baseline foundation and live-update time box

Covers the timetable's lab days of 2–20 Nov, which you are building yourself from 23 Nov (dates in `docs/catch-up-plan.md`). Written 22 Nov 2026 with AI assistance as a **guide**: steps, key settings, documentation to read and checks. It deliberately does not contain finished configs, so the build effort stays yours for RQ4.

Pinned versions: `versions.md`. Design: `docs/lab-design.md`. Permissions: `configs/shared/manifest.yaml`. Expected test outcomes: `docs/s1-test-checklist.csv`.

## Before you start: logging for RQ4

Write one row in `diary/effort_diary.csv` per work session, the same day, with the real date and real active minutes. Categories for this phase:

| Work | Category |
|---|---|
| Compose stack, networks, PKI, Keycloak, Envoy TLS/JWT, resources A–C | shared |
| Envoy RBAC rules, file-based update path, control utility, time box | local-permission |
| Network rules for Restricted (next phase) | network-restriction |
| OPA (December) | OPA |

Test scripts are measurement work, not configuration: log them as `shared` and start the task text with `harness:` so they can be separated in the RQ4 analysis. Commit at the end of each session and put the commit hash in the diary row. Count every config change that needed another try as a revision, and write each fault in a few words.

## Host preparation (once, before Day 1)

1. Linux VM or machine with 4 vCPUs, 16 GB RAM, 50 GB free (proposal 4.5). Record OS, kernel, Docker Engine and Compose versions in `versions.md`.
2. Install Docker Engine and the Compose plugin; add your user to the `docker` group.
3. Clone the repo on the host. Pull the pinned images and confirm their digests: `docker buildx imagetools inspect envoyproxy/envoy:v1.39.3` (compare with `versions.md`); then pin Keycloak's digest the same way.
4. Check the host cannot be reached from campus networks: no published ports in Compose, VM network in NAT or host-only mode.
5. Set up encrypted storage for `data/raw` and a backup location (timetable 26 Oct task).

## Day 1: isolated stack (timetable 2 Nov)

**Goal:** three Docker networks with empty services that start cleanly.

- In `scripts/lab/` create the Compose file with networks `client`, `resource` and `management`, each `internal: true` so containers have no route out.
- Add placeholder services (e.g. a small Alpine container per role: gateway, keycloak, resource-a/b/c, internal-client, probe-a, loadgen) attached to the networks in `docs/lab-design.md`. Publish no ports.
- Read: Docker Compose networking docs (`networks`, `internal`).

**Done when:** `docker compose up -d` starts everything; from `internal-client`, a container on `management` is unreachable, and `ping 1.1.1.1` fails from every container.

**Pitfall:** `internal: true` blocks all outbound traffic from containers, so `apk add` or `pip install` inside a running container fails. Image pulls are not affected (the Docker daemon pulls on the host's network). Put any tools a container needs into its image instead.

## Day 2: PKI (timetable 3 Nov)

**Goal:** a private CA, a server certificate for the gateway, and client certificates for the registered devices.

- Write `scripts/lab/pki.sh` with `openssl`: CA key and self-signed CA certificate; gateway server key and certificate with a SAN for its Compose hostname; one client certificate per device, `CN=dev-alice`, `CN=dev-bob` (plus admin's device once you decide, see the open point in the manifest).
- Put the identity you want Envoy to match into the certificate deliberately (CN or a SAN). Decide which and note it in the decisions log, because RBAC will match on it (Day 7).
- Keep keys out of Git: `.gitignore` already ignores `*.key` and `*.pem`. The script regenerates everything.

**Done when:** `openssl verify -CAfile ca.pem dev-alice.pem` and the gateway certificate both verify; SANs show in `openssl x509 -text`.

## Day 3: Keycloak realm and 30-second tokens (timetable 4 Nov)

**Goal:** a realm with Alice, Bob and admin; tokens with the right issuer, audience and 30-second lifetime.

- Run Keycloak `26.8.2` on the `management` network. Set the `hostname` option explicitly: Keycloak uses its configured hostname in the token issuer (`iss`), and Envoy must match that string exactly.
- Create the realm in the admin console: users `alice`, `bob`, `admin`; one client for the scripts with direct access grants enabled (password grant, acceptable for synthetic lab users; note it in Ch3); an audience mapper so `aud` contains the gateway's audience; realm access-token lifespan 30 s.
- Export the realm to `configs/shared/` (file name `<realm>-realm.json`) and import it at startup with `--import-realm` from `/opt/keycloak/data/import`, so every block starts from the same realm. Remove secrets from the export before committing, or keep it out of Git and regenerate.
- Read: Keycloak docs "Configuring the hostname (v2)" and "Importing and exporting realms".

**Done when:** a token fetched for each user decodes (e.g. with a local JWT decoder or `python -m jwt`) to the expected `iss`, `aud`, `preferred_username`, and `exp − iat = 30`.

## Day 4: Envoy downstream mTLS (timetable 5 Nov)

**Goal:** the gateway listener only accepts clients with a certificate from your CA.

- Envoy static bootstrap for now. On the gateway listener set a `DownstreamTlsContext` with `require_client_certificate: true`, the gateway certificate and key, and your CA as the trusted CA. Route to a test backend.
- Pin Envoy to CPU 1 and run it with one worker (`--concurrency 1`); this is part of the protocol.
- Read: Envoy TLS architecture overview and the `DownstreamTlsContext` proto (v1.39.3).

**Done when:** `curl --cacert ca.pem --cert dev-alice.pem --key dev-alice.key https://gateway/...` succeeds; the same request without a client certificate, or with one from another CA, fails in the TLS handshake.

## Day 5: JWT validation, header stripping, default deny (timetable 6 Nov)

**Goal:** only requests with a valid token reach any route.

- Add the `jwt_authn` HTTP filter. In the provider set `issuer` (exactly Keycloak's `iss`), `audiences`, the JWKS source (`remote_jwks` to Keycloak over the management network, or `local_jwks` from an exported key; write down which and why), `require_expiration: true`, `clock_skew_seconds: 1` (the default is 60 s), and `payload_in_metadata` so RBAC can read the verified claims later. Add a rule that requires this provider for every route prefix.
- Remove any client-supplied identity headers before routing (`request_headers_to_remove`), so a client cannot assert its own identity.
- Make unknown routes fail closed (no catch-all route to a backend).
- Read: Envoy "JWT Authentication (proto)" for v1.39.3, especially `require_expiration`, `clock_skew_seconds` and `payload_in_metadata`.

**Done when:** valid certificate + valid token gives 200; missing, expired or wrong-audience token gives 401; an unknown path is refused. Record the exact status codes you see in the checklist.

**Pitfall:** if `iss` differs by scheme, port or trailing slash from the provider's `issuer`, every token fails. Compare the strings byte for byte.

## Day 6: resources A, B, C behind guarded endpoints (timetable 9 Nov)

**Goal:** three resources with fixed synthetic responses, reachable only through Envoy.

- Each resource returns a fixed body with a unique content marker (e.g. `MARKER-A-7f3c`) so tests can detect forbidden content. A tiny static HTTP server per resource is enough.
- Give each resource a guarded Envoy endpoint (the "guarded endpoints" of the design) and keep the raw application ports off every network except the one Envoy uses to reach them.
- Read: `docs/lab-design.md` (which networks see which endpoints in Baseline).

**Done when:** each resource answers over TLS through its guarded endpoint; a port scan (`nc -z` from `internal-client` and `probe-a`) shows the raw application ports closed.

## Day 7: Baseline permissions with Envoy RBAC (timetable 10 Nov)

**Goal:** the manifest enforced locally: Alice→A, Bob→B, admin→C, GET only, device enabled and associated, default deny.

- Add the RBAC HTTP filter after `jwt_authn`, action `ALLOW` (everything not matched is denied).
- One policy per grant. Each policy combines: the user claim from the `jwt_authn` metadata (`metadata` principal), the device certificate identity (`authenticated` principal), the method `GET` and the resource path prefix (`header` and `url_path` permissions). Disabled devices are simply absent from the rules.
- Generate these rules from `configs/shared/manifest.yaml` with a small script rather than by hand; the same generator will matter for live updates (Day 10) and for the RQ4 fixed changes in January.
- Read: Envoy "Role Based Access Control (RBAC) (proto)", `Principal` and `Permission`. **[VERIFY]** which certificate field `authenticated.principal_name` matches against in v1.39.3 (SAN vs subject), and make your PKI match it.

**Done when:** the 3 users × 3 resources matrix gives exactly the three allows in the checklist; everything else is denied.

## Days 8–9: functional test script and fixes (timetable 11–12 Nov)

**Goal:** an automated S1 functional suite that Baseline passes completely.

- Write `scripts/measurement/s1_functional.py`: one test per row of `docs/s1-test-checklist.csv`, comparing actual status and content marker with the expected value; print a pass/fail table and exit non-zero on any failure.
- Fetch tokens outside any timed code; build the special tokens (expired, wrong audience, bad signature, no `exp`) as described in the checklist.
- Fix what fails and re-run until clean (Day 9).

**Done when:** the suite passes on Baseline twice in a row from a fresh `docker compose up`.

## Day 10: file-based config and control utility skeleton (timetable 13 Nov)

**Goal:** Envoy reads its listener/route config from files it watches, ready for the time box.

- Move Baseline's dynamic parts into files loaded through `dynamic_resources` with a filesystem `path_config_source`. Envoy only applies a new file when it is **moved** into place (atomic rename), not when edited in place. Mount the config directory (not single files) into the container and consider `watched_directory`, because a bind-mounted single file breaks rename detection.
- Decide where the RBAC rules live: in the listener (LDS update; listener drains) or per route (RDS update; no listener drain). Write the choice and reason in the decisions log, since it affects RQ2's reload interruptions.
- `scripts/control/ctl.py` skeleton: generate rules from the manifest, validate (e.g. `envoy --mode validate` on a bootstrap that loads the new file), write to a temp file, rename into place, log submission and acceptance timestamps (acceptance from Envoy's admin interface or logs on the management side).
- Read: Envoy "Configuration: Dynamic from filesystem" and `PathConfigSource` (v1.39.3).

**Done when:** Envoy starts from the files and a rename of an identical file is picked up without errors.

## Days 11–15: live-update time box (timetable 16–20 Nov; maximum five working days)

Same goals as the timetable; do not extend the box.

1. A rule change through `ctl.py` takes effect without restarting Envoy.
2. Withdraw Alice→A while Bob→B traffic continues; check enforcement on **new** connections; count reload interruptions as availability failures.
3. A malformed file is rejected and the accepted rules stay in force; timestamps are correct.
4. Ten withdraw/restore cycles in a row; if unreliable, simplify listener/control features.
5. Decide: reliable, simplified, or the proposal 4.5 fallback. Write it in the decisions log with the total time-box hours from the diary.

## When something does not work

Note the fault in the diary first, then debug. Envoy's admin interface (`/config_dump`, `/stats`, `/listeners`) on the management network shows what Envoy actually loaded. Ask me with the error message and the relevant config section, and I will help you find the cause, but you make the change.
