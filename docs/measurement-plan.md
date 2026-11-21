# Measurement plan and script list

Covers the timetable task of 29 Oct. Written 21 Nov 2026 from the proposal (Table 4, Section 4.2, Appendix A) with AI assistance. Statements marked **[VERIFY]** are assumptions about tool behaviour to confirm against the pinned versions in the pilot.

## Correlation and clocks

- Every request carries a request ID that appears in the client record, the Envoy access log, the resource log, firewall logs (where a rule fires) and, in Prototype, the OPA decision log. Envoy can generate `x-request-id` when the client sends none **[VERIFY: HTTP connection manager `generate_request_id` default in v1.39.3]**; the scripts should set their own ID so client and server records match even for rejected handshakes. OPA receives request headers in its input, so the ID can be logged with the decision **[VERIFY: decision-log fields in the pinned OPA-Envoy build]**.
- Client latency uses a monotonic clock (`time.perf_counter_ns`). Cross-component timestamps use wall-clock time; record the clock offset and error per block (pilot calibration).
- Token issuance and renewal (every 15 s in S1/S4) stay outside the timed interval.

## Per scenario

| Scenario | What is recorded | Timestamps needed | Block restore |
|---|---|---|---|
| S1 permissions and latency | expected vs actual decision, status, content marker, per-request latency (20 warm-up + 100 measured reads per configuration/block at 1 and 5 clients) | client send/receive (monotonic) | snapshots, credentials, permissions; balanced configuration order |
| S2 reachability | TCP connect, authenticated HTTP, forbidden content, 20 probes per source and endpoint; A's self-access excluded | probe time only | same block as S1 |
| S3 grant withdrawal | last success, first denial, three consecutive attributable denials; Bob→B availability; censoring at 20 s | control-utility submission and acceptance; probe sends/responses every 10 ms (20 ms if the pilot cannot sustain 10 ms); Envoy request start/end | restore Alice→A; fresh 30 s tokens issued 5 s before submission |
| S4 OPA outage | failure onset and window, readiness, first sustained success (three successes, no later failure); Restricted as control; 200 ms sampling bounds | fault injection, OPA readiness, every request at 5 reads/s per user | restart OPA, reload data, verify |
| Invalid update | malformed change rejected, accepted rules preserved (five paired repetitions) | submission, rejection | restore last accepted rules |

## Scripts to write

| Folder | Script | Purpose |
|---|---|---|
| `scripts/lab` | `build.sh`, `reset.sh`, `snapshot.sh` | bring up the isolated stack, restore state between blocks |
| `scripts/lab` | `pki.sh` | private CA, server and device certificates |
| `scripts/control` | `ctl.py` | validate, apply atomically, log submission/acceptance (file swap or OPA data API) |
| `scripts/measurement` | `s1_functional.py` | S1 variants with expected decisions from the manifest |
| `scripts/measurement` | `s1_load.py` | S1 latency workload at 1 and 5 clients |
| `scripts/measurement` | `s2_reach.py` | S2 probes from internal client and compromised A |
| `scripts/measurement` | `s3_withdraw.py` | S3 withdrawal with 10 ms probes |
| `scripts/measurement` | `s4_outage.py` | S4 outage and recovery |
| `scripts/measurement` | `invalid_update.py` | malformed-update repetitions |
| `scripts/measurement` | `block_runner.py` | one command per block: restore, CPU check, run order, export, checksum |
| `analysis` | `analyse.py` | per-block p95, paired differences, exact Wilcoxon on (increase − 10 ms), Holm, exact sign test, block bootstrap, censored bounds |

## Settings to confirm in the pilot

- `jwt_authn`: `require_expiration: true`, `clock_skew_seconds: 1`; tokens without `exp`, and tokens expired beyond exp + 1 s, rejected in all configurations.
- `ext_authz`: deny when OPA is down or the response is malformed (`failure_mode_allow: false`) and an explicit timeout **[VERIFY defaults in v1.39.3]**.
- No decision caching in Prototype.
- Noise floor below 2 ms per concurrency level before H2 testing.
