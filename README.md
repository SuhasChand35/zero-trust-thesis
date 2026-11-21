# Resource-level Zero Trust access prototype vs perimeter baseline (M.Sc. thesis)

Suhas Chandan Jagadeesha, SRH Berlin University of Applied Sciences.
Supervisors: Prof. Dr. Reiner Creutzburg (primary), Navaneeth Shivananjappa, M.Sc. (secondary).

| Folder | What goes in it |
|---|---|
| `configs/shared` | Keycloak realm export, CA config, permission manifest (single source of truth) |
| `configs/baseline` | Envoy config with local RBAC |
| `configs/restricted` | Baseline clone plus direct-path network rules |
| `configs/prototype` | Restricted plus ext_authz, OPA policy (Rego) and data |
| `scripts/lab` | Build, reset and snapshot scripts for the isolated host |
| `scripts/control` | Versioned control utility for permission changes |
| `scripts/measurement` | S1–S4 request scripts, probes, block runner |
| `data/raw` | Raw logs and timings (not in Git; checksummed, encrypted backup) |
| `data/pilot` | Pilot runs (kept out of the main analysis) |
| `data/processed` | Cleaned per-block tables produced by `analysis/` |
| `analysis` | Analysis code, figures and tables; reruns from raw data with one command |
| `diary` | RQ4 effort diary (`effort_diary.csv`), decisions log, AI-use log |
| `references` | Checked bibliography (`references.bib`), DOI check report, Zotero import steps |
| `docs` | SRH thesis rules and other admin notes |
| `thesis` | LaTeX source of the thesis |

Pinned versions live in `versions.md`. Never commit secrets: keys, tokens and real credentials stay out of Git.

## Building the thesis PDF

```
cd thesis
latexmk -pdf main.tex
```

Needs a TeX distribution with biber and biblatex-apa (TeX Live or MiKTeX).

## Remote

Private GitHub repository: https://github.com/SuhasChand35/zero-trust-thesis
