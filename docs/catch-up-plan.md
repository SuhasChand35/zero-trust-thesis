# Catch-up plan from Mon 23 Nov 2026

**Status on 22 Nov:** the October desk work is done (21 Nov). The lab build has not started, so the lab days of 2–20 Nov and everything after them are about three weeks behind. This plan moves the pending lab days to the coming weeks, uses Saturdays as catch-up days (as the timetable already allows), and shows the knock-on effect on the gates. It is a proposal: change it, then update the tracker, and tell your supervisors the new gate dates.

Lab days follow `docs/lab-build-guide.md`. Second blocks are your own writing (Ch2 drafting was also scheduled in Oct/Nov).

## Lab foundation, time box, Restricted, Prototype

| Date | Main block (lab) | Second block (writing) | Done when |
|---|---|---|---|
| Mon 23 Nov | Guide Day 1 + Day 2: isolated stack and PKI | Ch2 2.1 draft, part 1: Zero Trust principles, SP 800-207 roles | Stack starts on three internal networks; certificates verify |
| Tue 24 Nov | Day 3: Keycloak realm, 30 s tokens | Ch2 2.1 draft, part 2: CISA maturity model | Tokens have the right iss, aud and 30 s lifetime |
| Wed 25 Nov | Day 4: Envoy downstream mTLS | Ch2 2.2 notes: ABAC and policy engines | Requests without a valid client certificate fail the handshake |
| Thu 26 Nov | Day 5: jwt_authn, header stripping, default deny | Ch2 2.2 draft | Valid token 200; missing/expired 401 |
| Fri 27 Nov | Day 6: resources A, B, C behind guarded endpoints | Ch2 2.3 draft: BeyondCorp, SP 1800-35, Fernandez & Brazhuk | Raw ports closed; guarded endpoints answer over TLS |
| Sat 28 Nov | Day 7: Baseline RBAC from the manifest | diary and design notes | 3 × 3 matrix gives exactly the three allows |
| Mon 30 Nov | Day 8: functional test script | Ch2 2.4 notes: Basta, Noel | Suite runs against Baseline |
| Tue 01 Dec | Day 9: fix until clean | Ch2 2.4 notes: Mushtaq, Nkinyili | Baseline passes the suite twice from a fresh start |
| Wed 02 Dec | Day 10: file-based config, control utility skeleton | diary and design notes | Envoy loads its config from watched files |
| Thu 03 Dec | Time box day 1/5: live rule change | Ch2 2.4 draft, part 1 | A rule change applies without restart |
| Fri 04 Dec | Time box day 2/5: withdraw Alice→A, Bob unaffected | Ch2 2.4 draft, part 2 | Withdrawal enforced on new connections |
| Sat 05 Dec | Time box day 3/5: malformed update rejected | Ch2 2.5 notes: RFC 7009/7662, SSF, CAEP | Invalid update rejected cleanly |
| Mon 07 Dec | Time box day 4/5: ten withdraw/restore cycles | Ch2 2.5 draft | Ten clean cycles, or simplification chosen |
| Tue 08 Dec | Time box day 5/5: decision, logged | diary: total time-box hours | Decision in the decisions log |
| Wed 09 Dec | Restricted: Baseline clone + direct-path network rules | diary | Restricted passes the suite |
| Thu 10 Dec | S2 reachability probe script | Ch3 notes: Baseline | Probe script runs |
| Fri 11 Dec | Run S2 on Baseline and Restricted; live updates on Restricted | Ch3 notes: Restricted | Fewer reachable endpoints in Restricted |
| Sat 12 Dec | Full verification of Baseline and Restricted | diary | Both pass everything |
| Mon 14 Dec | Tag configs in Git; write up the build | Ch2 2.6 draft: research gap | Configs tagged |
| Tue 15 Dec | **Gate 1** (was 30 Nov): Baseline and Restricted verified | decisions log entry | Gate 1 decision written down |
| Wed 16 Dec | Prototype: OPA-Envoy on CPU 2, ext_authz fail-closed | diary | Stopping OPA denies traffic |
| Thu 17 Dec | Rego policy from the manifest | diary | Prototype passes the suite |
| Fri 18 Dec | OPA data updates through the control utility | Ch3 notes: Prototype | Withdrawal enforced; malformed update rejected |
| Sat 19 Dec | S2 and full suite on Prototype; encrypted backup + restore test | diary | Prototype verified; backup restored once |

## Holiday period (laptop only, as planned)

Use the six laptop days (21–23 and 28–30 Dec, about 3 h each) to **write** the harness scripts that need no running lab: S1 load generator, S3 withdrawal script, S4 outage script, invalid-update script, block runner and analysis script v1. They get their dry runs in January. Ch1 draft moves into the first week of January as a second block.

## Knock-on effect on later gates (proposed)

| Gate | Timetable | Proposed | How |
|---|---|---|---|
| Gate 1: Baseline and Restricted verified | 30 Nov | 15 Dec | this plan |
| Gate 2: Prototype and harness ready | 15 Dec | about 8 Jan | Prototype by 19 Dec; scripts written over the holiday; logging/correlation and dry runs 4–8 Jan |
| Pilot A–C and pilot decisions | 16–22 Dec | about 11–14 Jan | unchanged content |
| Gate 3: protocol freeze | 7 Jan | about 15 Jan | |
| Gate 4: data lock | 29 Jan | about 5 Feb | trials 18 Jan – 4 Feb, Saturdays included |
| Analysis complete | 9 Feb | about 16 Feb | |
| Gate 5: complete draft to supervisors | 26 Feb | about 5 Mar | Ch1–Ch4 drafted alongside the lab, so only Ch5–Ch7 remain in February |
| Submission | 30 Mar | 30 Mar (unchanged) | the 1–15 Mar feedback and retesting window absorbs the shift |

If any gate slips again by more than two working days, apply the proposal's cut order (extra permission variants first, then five-client traffic). The Restricted arm, core checks, analysis and revision stay protected.

## Decide before Monday

- [ ] Accept or change this plan; then update the tracker (I can do this).
- [ ] Admin's device (manifest open point); needed by Day 7.
- [ ] Tell both supervisors the shifted gate dates in your next update.
