# Reference check, 21 Nov 2026

The proposal's reference list has **24 entries**, not 22 as the timetable says. All 24 were checked.
DOIs were resolved against the Crossref API (api.crossref.org/works/<DOI>); web sources were opened directly.

## DOIs (11 of 11 resolve)

| Key | DOI | Resolves | Matches the proposal? | Notes |
|---|---|---|---|---|
| basta2022 | 10.1109/NOMS54207.2022.9789888 | yes | yes | Authors, title, NOMS 2022, pp. 1–7, IEEE all match. |
| borchert2025 | 10.6028/NIST.SP.1800-35 | yes | yes | Crossref lists only the 5 NIST authors; the PDF cover lists 95. The proposal's first 19 names match the cover order exactly and it correctly ends with Smith, B. (APA 7 rule for 21+ authors). Final version published June 2025. The .bib now holds all 95 names. |
| fernandez2024 | 10.1016/j.csi.2024.103832 | yes | yes | Vol. 89, article 103832, April 2024. |
| hu2014abac | 10.6028/NIST.SP.800-162 | yes | yes | Crossref date January 2014. "Includes updates as of August 2, 2019" comes from the document itself, not Crossref. |
| rfc7009 | 10.17487/RFC7009 | yes | yes, with a nuance | Crossref records T. Lodderstedt as *editor* and S. Dronia, M. Scurtescu as authors (RFC header: "T. Lodderstedt, Ed."). Citing Lodderstedt first is still correct. Aug 2013. |
| mushtaq2025 | 10.3390/s25196118 | yes | yes | Sensors 25(19), article 6118, published 3 Oct 2025. |
| nkinyili2026 | 10.15439/2026F5993 | yes | yes | ACSIS Vol. 49, pp. 73–80, PTI. Editors not in Crossref; confirmed on the volume page (annals-csis.org/Volume_49/drp/5993.html). Crossref container title is "Communication Papers of the 21st Conference on Computer Science and Intelligence Systems (FedCSIS)", as in the proposal. |
| noel2023 | 10.1177/15485129211051386 | yes | yes | 20(1), 57–79. Online first 8 Oct 2021; issue year 2023 (cite 2023). |
| rfc7662 | 10.17487/RFC7662 | yes | yes, with a nuance | Crossref records J. Richer as *editor* (RFC header: "J. Richer, Ed."). Oct 2015. |
| rose2020zta | 10.6028/NIST.SP.800-207 | yes | yes | Rose, Borchert, Mitchell, Connelly; published 11 Aug 2020. |
| wilcoxon1945 | 10.2307/3001968 | yes | yes | Biometrics Bulletin 1(6), Dec 1945; Crossref gives start page 80 only, 80–83 is correct. |

## Web sources without DOIs (13 of 13 load)

| Key | Checked | Notes |
|---|---|---|
| cappalli2025caep | loads | OpenID CAEP 1.0, 29 Aug 2025, authors Cappalli and Tulshibagwale. |
| tulshibagwale2025ssf | loads | OpenID SSF 1.0, 29 Aug 2025, six authors in the proposal's order. |
| cisa2023ztmm | URL valid | Direct fetch was blocked by cisa.gov, but the PDF is indexed at this URL: ZTMM Version 2.0, April 2023. |
| envoy_dynamic_fs, envoy_ext_authz, envoy_jwt_authn, envoy_rbac | load | All four pages are the "envoy tag-v1.39.3" docs. |
| opa_decision_logs, opa_envoy_plugin, opa_envoy_performance, opa_rest_api | load | Titles match the proposal. |
| keycloak_oidc | loads | "Securing applications and services with OpenID Connect". |
| ward2014beyondcorp | loads | ;login: Dec 2014, 39(6); the page shows no page numbers, 6–11 is from the print issue. |

## Problems found

1. **Holm (1979) is missing from the proposal's reference list.** H2 uses the Holm adjustment, so the thesis must cite it. Added to `references.bib` as `holm1979` (Scandinavian Journal of Statistics 6(2), 65–70; no DOI, JSTOR stable 4615733).
2. **The timetable says 22 references; the proposal has 24.** Use 24 (25 with Holm).
3. The analysis plan also uses an exact sign test and block-bootstrap intervals with no source. [CITATION NEEDED – verify] before Ch4 §4.5 is written: a standard nonparametric statistics text for the sign test, and a block-bootstrap reference.
4. Web sources were last read on 12 Oct 2026 per the proposal. Update `urldate` whenever you re-read a page for the thesis.

5. SSF 1.0 and CAEP 1.0 are Final Specifications; the .bib now points to the stable `-final` pages instead of the proposal's URLs (same date and authors). See `docs/literature-update-2026-11.md`.
