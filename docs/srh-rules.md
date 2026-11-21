# SRH thesis rules: what is known and what still needs checking

Last updated 21 Nov 2026. SRH's binding documents (the *Guidelines for Writing Academic Papers*, the *Thesis Template*, the examination regulations and any AI declaration form) are on eCampus behind the student login, so this page separates what public SRH pages say from what must still be read on eCampus or confirmed with the examination office.

## What public SRH pages say

| Topic | What SRH says publicly | Source (accessed 21 Nov 2026) |
|---|---|---|
| Citation style | The bibliography should follow APA style. | SRH University, "Academic Research: Definition and Fundamentals Explained" (srh-university.de/en/insights/research-and-projects/academic-research-definition-and-fundamentals-explained/) |
| Structure | The *Guidelines for Writing Academic Papers* give table-of-contents structures for a systematic criteria-based review, a systematic literature analysis, or an empirical study. This thesis is the empirical type. | same page |
| Templates | SRH provides pre-formatted "Outline Template" and "Thesis Template" files (on eCampus). | same page |
| Style | Objective, factual style; avoid the first-person "I" and the impersonal "one". | same page |
| AI use | Using AI for ideas or structure is generally permitted but must be disclosed; citing sources and disclosing any AI software used are mandatory. | same page |
| Credits | M.Sc. Computer Science – Cyber Security curriculum lists "Master's Thesis and Defence" at 20 ECTS (with Scientific Work 5 and Cyber Security Management Project 5 in the final semester). | srh-university.de/en/master/computer-science-cyber-security/i/ |

## Not found publicly (check on eCampus or ask the examination office)

- [ ] **Length:** word or page limit for this programme. Planning figure until confirmed: 15,000–20,000 words (project assumption, not an SRH rule). Note: one other SRH programme (Heidelberg, Water Technology) states about 20,000 words; that does not apply here.
- [ ] **Formatting:** font, size, line spacing, margins, page numbering, title page layout. Does the SRH *Thesis Template* have to be used, or may the thesis be written in LaTeX with the same layout?
- [ ] **APA edition:** APA 7 assumed (the proposal uses it).
- [ ] **AI use:** exact declaration wording or form, and whether a full prompt log must be submitted with the thesis.
- [ ] **Declaration of independent work:** exact wording.
- [ ] **Submission:** digital only or printed and bound copies (how many), where to submit, plagiarism check.
- [ ] **Writing period:** official registration date and deadline (planning: deadline 31 Mar 2027).
- [ ] **Defence/colloquium:** format and timing.
- [ ] **Ethics/data retention:** which step a synthetic-data lab study needs.

## What this means for the template now

- `thesis/main.tex` keeps APA 7 via biblatex-apa. That matches the public SRH statement.
- Write "this thesis" or passive voice rather than "I": the public SRH style advice says to avoid "I". Confirm with Prof. Creutzburg if you prefer first person.
- Margins (2.5 cm) and 1.5 spacing in `main.tex` are placeholders until the SRH template is read.
- Keep `diary/ai_use_log.md` up to date from now on; it feeds the AI declaration.
