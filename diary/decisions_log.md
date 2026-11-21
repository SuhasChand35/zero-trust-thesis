# Decisions log

One entry per decision that changes the design, scope, protocol or analysis. Newest at the bottom.

## Template

### YYYY-MM-DD: [short title]
- **Decision:**
- **Why:**
- **Alternatives considered:**
- **Affects:** (configurations, scenarios S1–S4, hypotheses H1/H2, RQ1–RQ4, manifest)
- **Agreed with supervisor:** yes / no / not needed

---

Entries before 21 Nov 2026 were logged retrospectively from the proposal revisions and the timetable; dates are those documents' dates.

### 2026-10-13: Three configurations instead of two
- **Decision:** Compare Baseline (local permissions), Restricted (Baseline plus network restrictions, no OPA) and Prototype (Restricted plus OPA).
- **Why:** Restricted separates the effect of network restrictions from the effect of the external policy path.
- **Alternatives considered:** Two configurations (perimeter baseline vs ZTA prototype) as in the earlier proposal revision.
- **Affects:** all configurations; S1–S3 include all three, S4 uses Restricted as outage control; H2 compares Prototype with Restricted; RQ1–RQ4.
- **Agreed with supervisor:** proposal submitted before 15 Oct 2026

### 2026-10-13: Envoy JWT expiry settings
- **Decision:** `require_expiration: true` and `clock_skew_seconds: 1` in every configuration, replacing the 60-second default skew.
- **Why:** 30-second tokens make a 60-second skew meaningless; S3 needs token expiry to be tight and identical across configurations.
- **Alternatives considered:** Envoy defaults.
- **Affects:** configs/shared, all configurations, pilot check A, S3.
- **Agreed with supervisor:** part of the proposal

### 2026-10-13: Fallback if Baseline live updates stay unreliable
- **Decision:** If local live updates are still unreliable at the protocol freeze, RQ2 reports Prototype observations without a paired speed claim; if only Baseline fails, keep Restricted and Prototype and document the missing local control as RQ4 evidence.
- **Why:** Protects the core comparison from one failing update path.
- **Alternatives considered:** Extending the 16–20 Nov time box (rejected: no extension).
- **Affects:** RQ2, RQ4, S3, Gate 1.
- **Agreed with supervisor:** part of the proposal (§4.5)

### 2026-11-20: Timetable runs from 15 Oct 2026
- **Decision:** Plan at normal pace from the thesis start on 15 Oct 2026 to the deadline on 31 Mar 2027; catch up on late work with Saturdays and extra hours; planned submission 30 Mar 2027.
- **Why:** Work started later than the planned 1 Oct.
- **Alternatives considered:** Finishing by 10 Mar 2027 (dropped).
- **Affects:** all gates; see the project timetable.
- **Agreed with supervisor:** not needed

### 2026-11-21: Reference library is 24 entries plus Holm (1979)
- **Decision:** Build the library from the 24 entries in the proposal (not 22) and add Holm (1979), which H2 uses but the proposal does not list. All 11 DOIs checked against Crossref.
- **Why:** Every method the analysis names needs a source.
- **Alternatives considered:** none
- **Affects:** references/, Ch2, Ch4 §4.5
- **Agreed with supervisor:** not needed

### 2026-11-21: Citation style APA 7 (provisional)
- **Decision:** Keep APA 7 (as in the proposal), set in `thesis/main.tex` via biblatex-apa.
- **Why:** The public SRH guidance says bibliographies follow APA.
- **Alternatives considered:** IEEE, Harvard.
- **Affects:** thesis/, references/
- **Agreed with supervisor:** [to confirm once the SRH guidelines on eCampus are read]

## Open questions

- [ ] SRH length limit, formatting rules, AI declaration wording and submission format (see `docs/srh-rules.md`).
- [ ] Must the SRH Word *Thesis Template* be used, or is a LaTeX thesis with the same layout accepted?
- [ ] Sources for the exact sign test and the block bootstrap (Ch4 §4.5).
- [ ] Outcome of the 16–20 Nov local-update time box (write the decision here).
