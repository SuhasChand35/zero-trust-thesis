# Literature update and standards check, 21 Nov 2026

Covers the timetable tasks of 21–22 Oct (literature updates 1 and 2) and the check that the revocation standards are still current. Done on 21 Nov 2026 with AI-assisted web search (Claude); see `diary/ai_use_log.md`.

**Important for the method chapter:** these were general web searches, not database searches. Before Ch4 cites a search strategy, re-run the strings below in IEEE Xplore, ACM Digital Library and Google Scholar yourself and record date, database and hit counts in the table at the end.

## Searches run

| # | Date | Where | Search string | Purpose |
|---|---|---|---|---|
| 1 | 2026-11-21 | web search | `zero trust testbed experimental evaluation latency 2026` | ZT testbeds and experimental evaluations 2025–26 |
| 2 | 2026-11-21 | web search | `Open Policy Agent Envoy ext_authz latency evaluation paper` | OPA / policy-as-code and ext_authz performance |
| 3 | 2026-11-21 | web search | `NIST SP 800-207A zero trust access control cloud-native applications service mesh` | NIST guidance closer to the Envoy/OPA design |
| 4 | 2026-11-21 | web search | `NIST SP 800-204B attribute-based access control microservices service mesh` | ABAC with a service mesh |
| 5 | 2026-11-21 | web search | `continuous access evaluation CAEP token revocation latency evaluation 2025` | Revocation and continuous access evaluation |
| 6 | 2026-11-21 | web search | `OpenID Foundation approves Shared Signals Framework 1.0 CAEP 1.0 final specifications` | Standard status |

## Candidates to read (not yet in the bibliography)

| Candidate | Why it may matter | Status |
|---|---|---|
| Chandramouli, R., & Butcher, Z. (2023). *A Zero Trust Architecture Model for Access Control in Cloud-Native Applications in Multi-Location Environments* (NIST SP 800-207A). https://doi.org/10.6028/NIST.SP.800-207A | NIST's own guidance for identity-based (not network-based) segmentation with API gateways and sidecar proxies. Very close to the Envoy design; Ch2 2.1/2.3 currently jump from SP 800-207 straight to deployments. | **Recommend adding.** Final, Sept 2023. Read Sections on policy enforcement. |
| Chandramouli, R., Butcher, Z., & Chetal, A. (2021). *Attribute-based Access Control for Microservices-based Applications Using a Service Mesh* (NIST SP 800-204B). https://doi.org/10.6028/NIST.SP.800-204B | Deployment guidance for ABAC with mutual authentication in a service mesh: bridges Hu et al. (2014) and the OPA prototype. | **Recommend adding.** Final, Aug 2021. |
| Farkiani, B., Liu, F., & Crowley, P. (2025). Enabling network policy enforcement in service meshes. arXiv:2510.04052 | Adds L3 policy enforcement to Istio; reports under 1 ms added latency; uses Envoy 1.33.3. Comparable "network restriction vs proxy policy" question. | Preprint (v2, 4 Nov 2025). Cite only as preprint, or check for a published version. |
| Brandner, L., et al. (2026). Enforcing application-layer policies in eBPF. arXiv:2605.31084 | Measures Envoy's per-request cost on their testbed (about 3.4 ms of a 7.2 ms average at 1,500 req/s). Useful context for H2's 10 ms budget. | Preprint (v4, 13 Aug 2026). Context only; different load and hardware. |
| Alayed, W., Tahir, H. A., & Hassan, W. U. (2026). End-to-end performance analysis of an intelligent zero-trust framework for SDN. *Electronics, 15*(11), 2376. https://doi.org/10.3390/electronics15112376 | Measured ZT latency on a VM testbed (about 83 ms mean authentication path under normal load, rising under DDoS). Shows how other ZT evaluations report latency. | Peer-reviewed; different architecture (SDN + deep learning). Optional for Ch2 2.4. |
| Gambo, M. L., & Almulhem, A. (2025). Zero trust architecture: A systematic literature review. arXiv:2503.11659 (journal DOI listed: 10.1007/s10922-025-09998-x) | Second recent SLR to set beside Mushtaq et al. (2025). | Verify the journal version before citing. |
| Microsoft. Continuous access evaluation in Microsoft Entra (Microsoft Learn) | Vendor CAEP implementation documents near-real-time critical events but up to 15 minutes propagation. Good Discussion point for RQ2: event-driven revocation still has delay. | Vendor documentation; cite with retrieval date if used. |

Screened out: ZT papers on 5G, quantum-resilient trust, federated learning, edge TEEs and LLM-agent enforcement (different problem, no comparison with a matched baseline), and an Authorea preprint on AI-driven poly-cloud ZT (not peer reviewed, little method detail).

## Standards check (21 Nov 2026)

| Standard | Status found | Action |
|---|---|---|
| RFC 7009 OAuth 2.0 Token Revocation | Proposed Standard (Aug 2013); IETF datatracker shows no "Updated by" or "Obsoleted by" | none |
| RFC 7662 OAuth 2.0 Token Introspection | Proposed Standard (Oct 2015); no "Updated by" or "Obsoleted by"; errata page lists reported errata | glance at the errata before quoting details |
| OpenID SSF 1.0 | Final Specification; vote closed late Aug 2025, OpenID press release 16 Sep 2025; final text at `openid-sharedsignals-framework-1_0-final.html` (published 29 Aug 2025) | **URL in references.bib switched to the `-final` page** |
| OpenID CAEP 1.0 | Final Specification, same vote; final text at `openid-caep-1_0-final.html` (29 Aug 2025). A separate CAEP Interoperability Profile was still in review in Sep 2026 (not final) | **URL switched to the `-final` page** |
| NIST SP 800-207 | Still the base document; companion SP 800-207A (Sep 2023) found, no revision of 800-207 itself found in this search | consider adding 800-207A (above) |

## Your database searches (fill in)

| Date | Database | Search string | Hits | Screened in | Notes |
|---|---|---|---|---|---|
| | IEEE Xplore | | | | |
| | ACM Digital Library | | | | |
| | Google Scholar | | | | |
