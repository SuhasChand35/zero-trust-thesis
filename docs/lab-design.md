# Lab design on paper

Covers the timetable task of 28 Oct (topology, CPU sets, permission manifest, threat-model table). Written 21 Nov 2026 from the proposal (Sections 4.1, 4.2, 4.5, Table 3, Appendix A) with AI assistance. Items marked **[PROPOSED]** are design choices not fixed in the proposal; confirm or change them and log the decision.

## Components

| Component | Role (NIST SP 800-207 term) | Present in |
|---|---|---|
| Keycloak | identity provider; issues 30-second JWT access tokens | all |
| Private CA | issues server certificates and client certificates for the registered devices | all |
| Envoy gateway | PEP: downstream mTLS, `jwt_authn`, header stripping, default-deny routes; guards endpoints A, B, C | all |
| Envoy RBAC rules | policy decision from the shared manifest (local, in the proxy) | Baseline, Restricted |
| OPA (OPA-Envoy plugin) | PE: decides every request via `ext_authz` over gRPC; no decision caching; deny if unavailable or malformed | Prototype |
| Control utility | approximates the PA: validates and applies permission changes, logs submission and acceptance times | all (file swap for Baseline/Restricted, OPA data API for Prototype) |
| Resources A, B, C | fixed synthetic read-only responses; raw application ports not reachable | all |
| Internal client, compromised-A probe | attacker positions from the threat model | all |
| Load generator and probes | S1–S4 traffic, monotonic timing | all |

## Networks **[PROPOSED addressing]**

| Network | Members | Proposed subnet |
|---|---|---|
| client | external requester, internal client, load generator, Envoy gateway listener | 10.10.1.0/24 |
| resource | Envoy guarded endpoints, resources A/B/C, compromised-A probe (in A's namespace) | 10.10.2.0/24 |
| management | Keycloak admin, control utility, OPA (Prototype), log collection | 10.10.3.0/24 |

No network has a route to campus or production networks (proposal 4.5). Raw application ports and management APIs are isolated in every configuration.

## What differs between configurations

| | Baseline | Restricted | Prototype |
|---|---|---|---|
| Authentication, mTLS, tokens, manifest | same | same | same |
| Permission decision | Envoy RBAC (local) | Envoy RBAC (local) | OPA via ext_authz |
| Internal client → guarded B/C directly | reachable (permissions still apply) | blocked by network rules | blocked |
| Compromised A → guarded B/C | reachable (permissions still apply) | blocked | blocked |
| Gateway path | open | open | open |
| Live permission update | Envoy file replacement | Envoy file replacement | OPA data API |
| OPA CPU set | reserved, idle | reserved, idle | used |

Restricted minus Baseline isolates the network restrictions; Prototype minus Restricted isolates the external policy path. The design does not estimate a full network × engine interaction (proposal 4.1).

## CPU pinning (from the proposal, provisional)

| CPU | Process |
|---|---|
| 0 | load generator / probes |
| 1 | Envoy (worker concurrency 1) |
| 2 | OPA (kept reserved in Baseline and Restricted) |
| 3 | resources, Keycloak, management, logging |

Record topology and affinity, and verify allocations before each block. Token issuance stays outside timed requests.

## Permission manifest

See `configs/shared/manifest.yaml`: Alice→A, Bob→B, admin→C, GET only, each subject to a valid token, a valid client certificate and an enabled, associated device; everything else denied. **Open point:** the proposal names two registered devices, but admin→C also needs one (decide before Gate 1).

## Threat model (proposal Table 3, paraphrased)

| Attacker position | Capability | Expected response |
|---|---|---|
| External requester | sends requests with missing, invalid, or valid but unpermitted credentials | all configurations deny invalid identity and forbidden content; permitted reads succeed |
| Internal client | holds Alice's valid user and device identity; probes guarded endpoints directly | Baseline allows the connection but checks permissions; Restricted and Prototype block the direct path |
| Compromised resource A | probes B and C from A's namespace; no management credentials | Baseline exposes guarded B/C endpoints (permissions still apply); Restricted and Prototype block lateral paths |

Trusted: identity provider, CA, host, management utility. Compromise of A is an initial condition (no exploitation). A stolen authorised identity can still perform allowed actions.

## Out of scope (state in Ch1 and Ch6)

Device-health attestation, high availability, fleet management, production migration, termination of already-open streams, write operations.
