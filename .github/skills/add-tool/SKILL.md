---
name: add-tool
description: "End-to-end workflow to fold a new supportive tool into the STM32Cube Benchmark PFE report. Use when the author says 'here is a tool', 'I built a script/harness/dashboard/parser to help the benchmarks', 'add this supportive tool', or describes a parallel development that enhances the benchmarking effort rather than being measured itself. Drives analyze → place → draft → visualize → cite → typeset → review, editing the LaTeX files directly and keeping the report coherent, humanized, cited, and compilation-ready."
argument-hint: "Path to the tool subproject (folder/files), plus a note on what it automates or enhances."
---

# Add a Supportive Tool to the Report

The companion workflow to **add-benchmark**. It takes a tool the **author designed and built
themselves** to support the benchmarks — automation, a measurement/logging harness, a
parser/aggregator, a dashboard, a flashing/orchestration utility, a CI pipeline, a config
generator — and produces a finished, cited, compile-ready report section that documents the
**engineering as the author's own contribution**, not benchmark numbers. Run it through the
**pfe-report-manager**, delegating each stage to the matching subagent.

## When to use
- The author delivers a tool **they built themselves** to support the benchmarks, which is not a
  measured experiment.
- A previously documented tool changed and its section must be regenerated.
- These are the author's own developments — not third-party tools merely used (those are only
  *cited* where relevant). Not a benchmark? Use this. Measured experiment producing metrics? Use
  **add-benchmark** instead.

## Inputs to confirm first
- **Tool location** (e.g. `tools/<name>/`) and which files are the entry points / docs.
- **Report root** — the folder whose `main.tex` contains `\begin{document}`
  (currently `INPT_PFE_Report_Template/`).

## Procedure

1. **Analyze** — delegate to `tool-analyst`.
   Produce the Tool Brief: problem solved, capabilities, architecture, tech stack, inputs/outputs,
   integration with the benchmark workflow, usage, engineering value, suggested visuals, external
   facts needing citation, and gaps. Resolve blocking gaps with the author before writing.

2. **Place** — delegate to `report-architect`.
   Supportive tools usually live in a **methodology / benchmarking-framework / tooling** chapter or
   a dedicated "Supporting tools developed" section — not interleaved with per-benchmark results.
   Search for existing coverage; choose the target chapter/section/subsection; create the heading
   skeleton with `\label{}`s; wire any new `\input{}` into `main.tex`; cross-reference the
   benchmarks that *use* the tool. Use [the tool subsection template](./assets/tool-subsection-template.tex)
   as the starting skeleton.

3. **Draft** — delegate to `technical-writer`.
   Write humanized prose in the author's engineering **"we"** ("we designed and built…") along the
   **tool arc**: *problem → design/architecture → implementation → integration → usage →
   value/outcome*. Present it as a PFE contribution, not a review of an off-the-shelf product.
   Motivate the tool by the benchmarking pain point it removes; describe the design without dumping
   the whole source; state the concrete improvement (reliability, reproducibility, time saved)
   honestly, with limitations. Insert `\cite{}`/`\ref{}` placeholders; collect acronyms and sources.

4. **Visualize** — delegate to `diagram-designer`.
   Tools are best explained visually: an **architecture diagram** (components + data flow) and/or a
   **workflow/pipeline diagram** (input → processing → output), plus an I/O or configuration table.
   Caption + `\label{}` each; place image files under `Figures/<name>/`. (See **generate-diagram**.)

5. **Cite** — delegate to `bibliography-manager`.
   The tool itself is the author's contribution and is **not** cited; cite the external libraries,
   frameworks, and vendor tools it builds on (e.g. STM32CubeProgrammer, OpenOCD, `pyserial`),
   standards, and any referenced technique. Add `\bibitem` entries in `main.tex`'s `thebibliography`
   and insert `~\cite{}` at the supporting sentence; audit for uncited claims. (See **check-bibliography**.)

6. **Typeset** — delegate to `latex-engineer`.
   Assemble prose + diagrams + citations into the target `.tex` file(s); add acronyms to
   `Endmatter/Acronyms.tex`; push long code/config listings to `Endmatter/ComplementaryCodes.tex`
   (use `\lstset{style=mystyle}`) and reference them, so the narrative stays readable. Keep
   everything pdfLaTeX-clean. (See **validate-latex**.)

7. **Review** — delegate to `quality-reviewer`.
   Run the consistency/quality checklist. Apply blocking and consistency fixes (via writer /
   latex-engineer / bibliography-manager) before declaring done.

8. **Coherence sweep.**
   Make sure the benchmark sections that rely on this tool reference it (`\ref{}`) instead of
   re-explaining it, and that the methodology narrative now accounts for the tool. If the tool
   changes how results were obtained, revise the affected methodology framing.

## Definition of done
- Content lives in the correct `.tex` file with meaningful labels; no `Lorem ipsum` left in it.
- The tool is documented as the author's own engineering contribution (problem → design →
  integration → value), in the "we built…" voice — not as a benchmark or a third-party tool review.
- The author's own tool is not cited; every external library/fact it relies on has a `\cite` backed
  by a real `\bibitem`; acronyms are defined and listed.
- Architecture/workflow diagrams (and any I/O table) are captioned, labeled, and referenced.
- Long code/config lives in the appendix and is referenced, not pasted inline wholesale.
- The section reads as humanized academic prose consistent with the rest of the report.
- The project still compiles under pdfLaTeX (or the exact remaining issue is reported).
- The manager's summary lists files changed, placement rationale, visuals, citations, coherence
  edits, and open `% TODO(source)` items.
