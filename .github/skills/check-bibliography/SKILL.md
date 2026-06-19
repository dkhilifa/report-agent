---
name: check-bibliography
description: "Audit and complete the bibliography of the STM32Cube Benchmark PFE report. Use when the author asks to 'check citations', 'verify sources', 'fix the bibliography', or before a submission milestone. Confirms every external claim has a \\cite backed by a real \\bibitem, finds uncited facts and dangling references, normalizes \\bibitem formatting, and reports gaps — without inventing sources."
argument-hint: "Optionally the section/file to audit; default is the whole report."
---

# Check Bibliography Completeness

Guarantee that nothing external is asserted without a verifiable, correctly formatted citation.
Delegate to the **bibliography-manager**.

## When to use
- After drafting/adding content that uses external facts.
- Before a review or submission, as a full-report audit.

## The template's system
- Manual `thebibliography` at the end of `main.tex`; `\bibitem{key} …` entries; `cite` package
  loaded so `\cite{a,b}` compresses. Citations use `~\cite{key}` at the supporting sentence.
- Keys are semantic and stable: `st-rm0433`, `st-an5083`, `coremark-eembc`, `stm32cubemx-um`.

## Audit procedure
1. **Collect external claims.** Scan the target scope for statements that depend on an outside
   source: datasheet/reference-manual figures, application-note guidance, standard/benchmark
   definitions (CoreMark, Dhrystone, EEMBC, ULPMark), vendor specs, third-party numbers, quotes.
2. **Check each has a `\cite`.** Flag any external claim with no citation as **uncited**.
3. **Resolve references both ways:**
   - every `\cite{key}` has a matching `\bibitem{key}` (no dangling cites);
   - every `\bibitem{key}` is cited at least once (no orphan entries);
   - no duplicate sources under different keys (merge them).
4. **Verify metadata.** Each `\bibitem` has organization/author, exact title (+ revision), URL, and
   access date where applicable. Fetch the source to confirm details rather than guessing.
5. **Normalize formatting** to one consistent IEEE-like style (see bibliography-manager templates).
6. **Replace placeholders.** Remove the template's `ref1`/`ref2` stubs once real sources exist.

## Hard rules
- **Never invent** an author, title, URL, date, or number. If a source can't be verified, mark the
  claim `% TODO(source)` and list it for the author — do not fabricate a `\bibitem`.
- Keep the manual system unless the author explicitly approves a BibTeX migration.

## Output — Audit Report
- **Uncited external claims** — file + quoted sentence → suggested source.
- **Dangling `\cite`** (no `\bibitem`) and **orphan `\bibitem`** (never cited).
- **Duplicate / inconsistent entries** — and the merge/normalization applied.
- **Metadata gaps** — entries missing URL/date/revision.
- **Fixes applied** vs **items left for the author** (`% TODO(source)`).
- One-line verdict: citation-complete, or the must-fix list.
