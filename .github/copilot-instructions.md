# PFE Report — Project Instructions

These instructions always apply in this workspace. They define how Copilot and the
custom agents help write the **PFE (Projet de Fin d'Études) report**.

## 1. Project identity

- **Report title:** *STM32Cube Ecosystem Benchmark*
- **Document type:** Engineering end-of-studies report (INPT PFE), graded academic deliverable.
- **Primary output format:** **LaTeX** (compiled with **pdfLaTeX**).
- **Audience:** Academic jury + industrial supervisor — readers are engineers, but not
  necessarily STM32 specialists. Explain domain terms on first use.
- **What the benchmark compares:** the **STMicroelectronics / STM32Cube ecosystem (the reference)**
  against **equivalent competing vendors** (e.g. GD32, Texas Instruments, NXP, Renesas). Every
  comparison is **ST vs one other vendor — never ST vs ST.** Each comparison targets a concrete
  **software stack** (USB device stack, FreeRTOS integration, etc.) running on a representative
  board from each vendor, exercised through several **scenarios**.
- **Scenario:** a concrete, reproducible use-case variant of a stack benchmark that both platforms
  must run identically. Example — the USB device stack is exercised through the **HID**, **CDC**,
  and **MSC** scenarios; a FreeRTOS benchmark might use context-switch, queue-throughput, and
  interrupt-latency scenarios. A stack is benchmarked *through* its scenarios; results are reported
  per scenario and then synthesized.
- **Working method:** The report is built **incrementally**. The author delivers subprojects one
  at a time; each is analyzed, placed, and written into the report.
- **Two kinds of subproject (deliverables):**
  1. **Benchmark subprojects** — measured experiments that produce metrics/results (the core
     contribution). Handled by the **add-benchmark** workflow.
  2. **Supportive tools** — software the author **designed and built themselves** to enhance the
     benchmarking effort (not third-party tools merely used, and not measured experiments):
     automation scripts, measurement/logging harnesses, result parsers and aggregators,
     comparison dashboards, flashing/orchestration utilities, CI pipelines, configuration
     generators, etc. Handled by the **add-tool** workflow.
  Both are first-class report content and count as the author's own engineering contribution.
  A tool is documented for the engineering it represents (problem solved, design, integration,
  value), not for benchmark numbers, and is written up as the author's work ("we designed and
  built…"). The author's own tools are **not** cited as external sources; only the external
  libraries, frameworks, and vendor tools they build on (e.g. pyserial, STM32CubeProgrammer) are.

## 2. Report location and structure

The LaTeX report currently lives in **`INPT_PFE_Report_Template/`** (the "report root").
If the report root is ever moved or renamed, locate it by finding the file that contains
`\begin{document}` (that is `main.tex`).

| Path | Role |
|------|------|
| `INPT_PFE_Report_Template/main.tex` | Master file: preamble hooks, `\include`/`\input` of all parts, **bibliography** at the end |
| `INPT_PFE_Report_Template/Packages.tex` | Package imports and global formatting |
| `INPT_PFE_Report_Template/Chapters/` | `Chapter1.tex` … `Chapter5.tex`, `Conclusion.tex` |
| `INPT_PFE_Report_Template/Frontmatter/` | Page de garde, Dedication, Acknowledgements, Résumé (FR), Abstract (EN), Molakhas (AR) |
| `INPT_PFE_Report_Template/Endmatter/` | `Glossary.tex`, `Acronyms.tex`, `ComplementaryCodes.tex` (appendices) |
| `INPT_PFE_Report_Template/Figures/` | Content figures (create sub-folders per topic) |
| `INPT_PFE_Report_Template/Logos/` | Institutional/company logos only |

**Section hierarchy:** `\chapter` → `\section` → `\subsection` → `\subsubsection` → `\paragraph`.
`secnumdepth`/`tocdepth` are set to `4`, so subsubsections are numbered and appear in the ToC.

**Per-vendor chapter organization.** The benchmark results are organized as **one dedicated
chapter per competing vendor** (e.g. *Benchmarking ST against GD32*, *Benchmarking ST against TI*).
Inside a vendor chapter the layout is fixed so every comparison reads the same way:

1. a short **introduction** (why this vendor, what the comparison should settle);
2. a **Hardware platforms** section comparing the two boards' characteristics (core, clock,
   flash/RAM, key peripherals, toolchain, price) in a `booktabs` table;
3. **one `\section` per benchmarked stack** (USB device stack, FreeRTOS, …), each structured as
   **scenario design → results (per scenario) → interpretation**;
4. a **conclusion** synthesizing how ST stands against that vendor.

Routing for new subprojects: a **new vendor** ⇒ a **new chapter** (new `Chapters/Chapter<N>.tex`
wired into `main.tex`); an **existing vendor + new stack** ⇒ a new `\section` in that vendor's
chapter; an **existing stack + new scenario** ⇒ a new results subsection. An optional final
**cross-vendor synthesis** chapter ranks ST against all competitors once several vendor chapters
exist.

## 3. Compilation facts (do not break these)

- Compiler is **pdfLaTeX**. Do **not** introduce packages or syntax that require XeLaTeX/LuaLaTeX
  (e.g. `fontspec`) without flagging it explicitly to the author first.
- Already available (see `Packages.tex`) — **prefer these, do not re-import**:
  `tikz` (+`calc`), `pgf`, `booktabs`, `longtable`, `tabularx`, `multirow`-style `m{}` columns,
  `algorithm2e`, `listings` (use the predefined `\lstset{style=mystyle}`), `glossaries`
  (acronyms), `hyperref`, `caption`, `float` (`[H]`), `cite`, `amsmath`/`amssymb`.
- If a new package is genuinely required, add it to `Packages.tex` only, near related imports,
  and note why in your summary to the author.
- Figures use `\includegraphics`; images go in `Figures/` (content) or `Logos/` (logos).
  `pgfplots` is **not yet imported** — if a plot needs it, add `\usepackage{pgfplots}` +
  `\pgfplotsset{compat=1.18}` to `Packages.tex` and tell the author.

## 4. Citations and bibliography (mandatory for external information)

The template uses a **manual bibliography**: a `thebibliography` environment at the end of
`main.tex` with `\bibitem{key} …` entries, cited in text with `\cite{key}`. The `cite`
package is loaded, so grouped numeric citations like `\cite{key1,key2}` compress correctly.

**Non-negotiable rules whenever external information is used** (a website, datasheet,
application note, paper, manual, blog, or any reference):
1. **Track the source** — capture author/organization, exact title, URL, and access date.
2. **Add a `\bibitem`** to the `thebibliography` block in `main.tex` with a descriptive key
   (e.g. `st-an5083`, `coremark-eembc`, `stm32h7-rm0433`).
3. **Insert the `\cite{key}`** at the precise sentence the information supports.
4. **Never state an external fact without a citation.** No "floating" claims, no invented
   numbers, no fabricated source titles or URLs. If a value is unverified, label it clearly
   as an assumption or leave a `% TODO(source)` marker rather than inventing a reference.

The detailed citation procedure and the optional one-time migration to BibTeX live in the
**`check-bibliography`** skill and the **bibliography-manager** agent.

## 5. Writing style — professional, academic, *humanized*

The report must read like careful engineering prose written by a competent student, not like
generated boilerplate. Apply the **`improve-style`** skill for deep edits; the essentials:

**Do**
- Write in a consistent voice. Default to the engineering **"we"** (e.g. "we measured…",
  "we observed…"); keep it consistent across every section already written.
- Motivate before measuring: say *why* a benchmark matters before presenting *how* and *what*.
- Interpret results — never drop a table or number without explaining what it means and the
  trade-off it reveals.
- Vary sentence length and structure; use logical connectors (however, consequently, in
  contrast, as a result) to build an argument.
- Define each acronym on first use, then add it to `Endmatter/Acronyms.tex`.
- Be precise with units and quantities (cycles, µs, KB of flash/RAM, mA, µJ, MB/s, CoreMark/MHz).

**Avoid**
- AI-tells and filler: "delve", "in the realm of", "it is worth noting", "plays a crucial
  role", "tapestry", "navigating the landscape", empty triads, restating the heading as the
  first sentence.
- Marketing tone or unverifiable superlatives ("blazingly fast", "the best").
- Inconsistent terminology — pick one term per concept (e.g. "HAL driver", not alternating
  "HAL"/"Hal layer"/"abstraction lib") and keep a running glossary.
- Duplicating content that already exists elsewhere in the report; cross-reference with
  `\ref{}`/`\label{}` instead.

## 6. Core operating rules

1. **Edit the report directly.** When a section is drafted or improved, write it into the
   correct `.tex` file under the report root — do not just paste LaTeX in chat.
2. **Preserve the document organization.** Reuse the existing chapter/section scaffold; place
   new content where it belongs (see the **report-architect** agent) rather than inventing
   parallel structures. Remove leftover `Lorem ipsum`/placeholder text only in the sections
   you are actively writing.
3. **No duplication.** Before writing, check whether the topic is already covered; if so,
   extend or cross-reference instead of repeating.
4. **Coherence first.** New content must match the tone, depth, terminology, and formatting of
   what already exists. When a new subproject changes the story, revise earlier sections so the
   narrative stays consistent.
5. **Keep labels meaningful.** Use `\label{chap:…}`, `\label{sec:…}`, `\label{fig:…}`,
   `\label{tab:…}`, `\label{lst:…}` so cross-references and the ToC stay clean.
6. **Stay compilation-ready.** Balanced braces/environments, escaped special characters
   (`\% \& \_ \# \$`), and valid references. The **`validate-latex`** skill checks this.

## 7. The agent + skill system

This project ships a coordinated team under `.github/`:

**Agents** (`.github/agents/`) — *who* does the work:
- **pfe-report-manager** — main orchestrator; entry point for "here is a new benchmark/tool".
- **benchmark-analyst** — extracts purpose, methodology, tools, metrics, results from a benchmark.
- **tool-analyst** — extracts purpose, design, integration, usage, and value from a supportive tool.
- **report-architect** — decides placement in chapters/sections; maintains structure and ToC.
- **technical-writer** — produces humanized academic prose.
- **latex-engineer** — converts prose to clean LaTeX, edits files, checks compile-readiness.
- **diagram-designer** — TikZ/pgfplots diagrams, tables, and figure suggestions.
- **bibliography-manager** — tracks sources, manages `\bibitem` entries and `\cite` calls.
- **quality-reviewer** — read-only consistency, style, and correctness review.

**Skills** (`.github/skills/`) — *how* the work is done (also available as `/` slash commands):
- **add-benchmark** — end-to-end workflow to fold a new benchmark subproject into the report.
- **add-tool** — end-to-end workflow to fold a new supportive tool into the report.
- **draft-section** — generate a first draft of a section/subsection.
- **improve-style** — raise clarity and humanization of existing prose.
- **check-bibliography** — verify every external claim is sourced and cited.
- **generate-diagram** — produce a diagram/figure/table from a description.
- **validate-latex** — structural and compile-readiness checks.

When the author says "here is a new benchmark subproject", run the **add-benchmark** workflow
through the **pfe-report-manager**. When they say "here is a supportive tool" (or describe a
parallel development that aids benchmarking), run the **add-tool** workflow instead.

## 8. Recommended repository organization

```
PFE-REPORT/
├─ .github/                      # this agent system (agents, skills, instructions)
├─ INPT_PFE_Report_Template/     # the LaTeX report (report root)
│  ├─ main.tex  Packages.tex
│  ├─ Chapters/  Frontmatter/  Endmatter/
│  ├─ Figures/                  # one sub-folder per benchmark/tool, e.g. Figures/coremark/
│  └─ Logos/
├─ benchmarks/                   # (suggested) incoming benchmark subprojects, one folder each
└─ tools/                        # (suggested) incoming supportive tools, one folder each
```

Keep each incoming subproject in its own folder (`benchmarks/<name>/` for benchmarks,
`tools/<name>/` for supportive tools) so the analysts can inspect sources, build outputs, and
result logs without polluting the report root.
