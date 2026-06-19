---
name: add-benchmark
description: "End-to-end workflow to fold a new STM32Cube benchmark subproject into the PFE report. Use when the author says 'here is a new benchmark', 'add this subproject', or 'integrate these results'. Each benchmark compares ST (the reference) against one competing vendor through a software stack and its scenarios; results live in one chapter per vendor. Drives analyze → place → draft → visualize → cite → typeset → review, editing the LaTeX files directly and keeping the report coherent, humanized, cited, and compilation-ready."
argument-hint: "Path to the benchmark subproject (folder/files), plus any notes on what it measures."
---

# Add a Benchmark Subproject to the Report

The flagship workflow. It takes a completed benchmark subproject and produces a finished,
cited, compile-ready report section in the right place. Run it through the **pfe-report-manager**,
delegating each stage to the matching subagent.

## When to use
- The author delivers a new benchmark subproject and wants it written into the report.
- A subproject was updated and its section must be regenerated.
- **Not a benchmark?** If the deliverable is a *supportive tool* that enhances the benchmarking
  effort but is not itself measured (automation, measurement/logging harness, parser/aggregator,
  dashboard, CI, config generator), use the **add-tool** skill with `tool-analyst` instead.

## Inputs to confirm first
- **Subproject location** (e.g. `benchmarks/<name>/`) and which files hold the results.
- **Report root** — the folder whose `main.tex` contains `\begin{document}`
  (currently `INPT_PFE_Report_Template/`).

## Procedure

1. **Analyze** — delegate to `benchmark-analyst`.
   Produce the Analysis Brief: purpose, the two vendors compared (ST reference vs competitor),
   board characteristics of both platforms, the stack under test and its scenarios, what it
   measures (+units), toolchain, methodology, per-scenario raw results, key trade-off, suggested
   visuals, external facts needing citation, and gaps. Resolve blocking gaps with the author
   before writing.

2. **Place** — delegate to `report-architect`.
   Apply the per-vendor routing: a **new vendor** ⇒ a new `Chapters/Chapter<N>.tex` (titled
   *Benchmarking ST against <Vendor>*) wired into `main.tex`; an **existing vendor + new stack**
   ⇒ a new `\section` in that vendor's chapter; an **existing stack + new scenario** ⇒ a new
   results subsection. Create the heading skeleton with `\label{}`s; list earlier sections that
   need revising for coherence. Use [the chapter template](./assets/benchmark-subsection-template.tex)
   as the starting skeleton.

3. **Draft** — delegate to `technical-writer`.
   Write humanized prose along the arc **motivation → scenario design → results (per scenario) →
   interpretation → takeaway**, keeping ST and the competitor side by side. Establish the
   board-characteristics comparison once at the top of the chapter. Insert `\cite{}`/`\ref{}`
   placeholders; collect acronyms and the source list. Match the voice and depth of existing
   sections. (See the **draft-section** skill for the section anatomy.)

4. **Visualize** — delegate to `diagram-designer`.
   Build the comparison table and/or chart and any architecture/workflow diagram the data warrants;
   caption + `\label{}` each; place image files under `Figures/<name>/`. (See **generate-diagram**.)

5. **Cite** — delegate to `bibliography-manager`.
   Turn the source list into `\bibitem` entries in `main.tex`'s `thebibliography`; insert `~\cite{}`
   at each supporting sentence; audit for uncited claims. (See **check-bibliography**.)

6. **Typeset** — delegate to `latex-engineer`.
   Assemble prose + visuals + citations into the target `.tex` file(s); add acronyms to
   `Endmatter/Acronyms.tex`; push exhaustive config/code to `Endmatter/ComplementaryCodes.tex` if
   it clutters the flow; keep everything pdfLaTeX-clean. (See **validate-latex**.)

7. **Review** — delegate to `quality-reviewer`.
   Run the consistency/quality checklist. Apply blocking and consistency fixes (via writer /
   latex-engineer / bibliography-manager) before declaring done.

8. **Coherence sweep.**
   If this benchmark changes the report's overall story (e.g. it overturns an earlier expectation),
   revise the affected earlier sections and the synthesis/conclusion so the narrative stays consistent.

## Definition of done
- Content lives in the correct `.tex` file with meaningful labels; no `Lorem ipsum` left in it.
- Every external fact has a `\cite` backed by a real `\bibitem`; acronyms are defined and listed.
- Figures/tables are captioned, labeled, and referenced from the prose.
- The section reads as humanized academic prose consistent with the rest of the report.
- The project still compiles under pdfLaTeX (or the exact remaining issue is reported).
- The manager's summary lists files changed, placement rationale, visuals, citations, coherence
  edits, and open `% TODO(source)` items.
