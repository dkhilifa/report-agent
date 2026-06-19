---
name: draft-section
description: "Generate a first draft of a report section or subsection for the STM32Cube Benchmark PFE report. Use when the author asks to 'draft', 'write up', or 'start' a section, or to scaffold a heading into full prose. Produces humanized academic LaTeX following the motivation → method → results → interpretation → takeaway arc (for a benchmark stack: motivation → scenario design → per-scenario results → interpretation → takeaway, ST vs the competing vendor), with citation and figure placeholders, written into the correct .tex file."
argument-hint: "The section/topic to draft, the target chapter, and any data or notes to base it on."
---

# Draft a Section

Produce a clean first draft of a section or subsection, ready for refinement. Delegate writing to
the **technical-writer** and typesetting to the **latex-engineer**; consult the **report-architect**
if the placement is not yet decided.

## When to use
- A heading exists (or is needed) and you want it turned into full, humanized prose.
- You are starting a new chapter/section and want a coherent skeleton-plus-draft.

## Section anatomy
Default to this arc; adapt for non-result sections (context, methodology, synthesis):
1. **Motivation** — why this topic/benchmark matters to the report's question. No heading echo.
2. **Method / description** — what was done or what the concept is, reproducibly but readably.
3. **Results / content** — the data, pointed to by `\ref{tab:…}` / `\ref{fig:…}`.
4. **Interpretation** — what it means, the trade-off, the practical guidance.
5. **Takeaway** — one or two sentences to remember.

For a **benchmark stack section** (ST vs a competing vendor), use the comparative variant:
motivation → **scenario design** (which scenarios exercise the stack and why) → **results per
scenario** (ST and the vendor side by side) → interpretation → takeaway. The board-characteristics
comparison is established once at the top of the vendor chapter, not repeated per section.

## Procedure
1. **Confirm placement & voice.** Identify the target `.tex` file and read sibling sections to
   match tone, depth, and terminology. (Use the report-architect if placement is open.)
2. **Gather substance.** Use the benchmark-analyst brief or the author's notes. Do not invent data;
   leave `% TODO(data)` where a number is missing.
3. **Write the draft** (technical-writer) following the arc. Humanized voice ("we"), varied
   sentences, acronyms defined on first use, units precise. Insert `\cite{}` / `\ref{}` placeholders.
4. **Scaffold visuals** — add captioned, labeled table/figure stubs where data will go; hand real
   figures to the **generate-diagram** skill.
5. **Write it into the file** (latex-engineer) in the right section; keep labels meaningful.
6. **Hand off** the source list to the **bibliography-manager** and acronyms to `Endmatter/Acronyms.tex`.

## Quality bar for a draft
- Reads like a person wrote it — no AI-tells, no heading restated as the first line.
- Every claim is either grounded in provided data or marked `% TODO`.
- External facts carry a `\cite{}` placeholder with the source noted for citation.
- Compiles (or only stubs/placeholders remain). Run **validate-latex** before finishing.

## Output
The drafted prose in the target `.tex` file, plus a short note of: placeholders left
(`% TODO`, `\cite{}`, `\ref{}`), acronyms introduced, and sources to cite.
