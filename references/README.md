# References

- `../thesis/references.bib`: the 24 proposal references plus Holm (1979), checked on 21 Nov 2026. This is the file to import. It sits in `thesis/` so the thesis folder works on its own in Overleaf.
- `doi-check.md`: what was checked for each entry and what differed.
- `dois.txt`: the 11 DOIs, one per line.

## Build the Zotero library (about 5 minutes)

1. In Zotero, create a collection called **ZTA thesis**.
2. **DOI items (11):** select the collection, click the magic-wand button (*Add Item(s) by Identifier*), paste all lines of `dois.txt` and press Enter. Zotero pulls the publisher metadata.
3. **Everything else (14):** *File > Import…* > `thesis/references.bib` > tick "Place imported collections and items into new collection", then drag the items into **ZTA thesis** and delete the 11 duplicates of the DOI items (keep the identifier-fetched copies).
4. Fix the two RFCs by hand if you want the editor role: Lodderstedt (RFC 7009) and Richer (RFC 7662) are listed as "Ed." in the RFC headers.
5. For SP 1800-35, Zotero may import only the 5 NIST authors from Crossref; keep the `.bib` copy instead (all 95 names) so the APA output ends with "… Smith, B.".
6. Set the citation style to APA 7th edition (see `docs/srh-rules.md` for the open check with SRH).

If you write in LaTeX, `thesis/main.tex` already reads this file with biblatex-apa, so Zotero is only needed for reading notes and PDFs. If you switch to Word, export from Zotero or use the Zotero Word plugin.
