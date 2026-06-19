---
name: improve-style
description: "Raise the clarity, professionalism, and humanization of existing prose in the STM32Cube Benchmark PFE report. Use when the author asks to 'improve', 'humanize', 'polish', 'rewrite', or 'make less robotic' a section. Removes AI-tells and filler, enforces a consistent 'we' voice and terminology, strengthens result interpretation, and keeps the academic register — without changing technical facts or citations."
argument-hint: "The section/file to improve (and any tone or length preference)."
---

# Improve Style and Clarity

Edit existing prose so it reads like careful engineering writing, not generated boilerplate —
while preserving every technical fact, number, and citation. Delegate to the **technical-writer**;
finish with a **quality-reviewer** pass.

## When to use
- A drafted section feels robotic, generic, repetitive, or inconsistent with the rest.
- Before review milestones, to lift a chapter to submission quality.

## Hard rules
- **Do not change facts.** Keep numbers, units, configurations, `\cite{}` keys, and `\ref{}` intact.
  Rephrase only; if a claim looks wrong, flag it — don't silently "fix" it.
- **Preserve LaTeX structure.** Don't alter labels, environments, or section levels while editing prose.

## What to fix (see the [style guide](./references/academic-style-guide.md) for the full list)
- **AI-tells & filler** — "delve", "in the realm of", "it is worth noting", "plays a crucial role",
  "tapestry", "navigating the landscape", hollow triads → cut or replace with substance.
- **Heading echo** — first sentence restating the heading → open with the actual point.
- **Monotony** — uniform sentence length/openings → vary rhythm; add logical connectors
  (however, consequently, in contrast, as a result).
- **Voice drift** — mixed person/tense → consistent engineering **"we"**, matching neighbors.
- **Terminology drift** — synonyms for one concept (HAL / Hal layer / abstraction lib) → one term.
- **Uninterpreted data** — a table/number with no meaning → add what it reveals and the trade-off.
- **Vague quantities** — "much faster" → exact figures with units (cycles, µs, KB, mA, µJ).
- **Marketing tone** — "blazingly fast", "the best" → measured, evidence-based phrasing.

## Procedure
1. Read the target section and a couple of neighbors to learn the established voice.
2. Apply the fixes above, editing in place. Keep edits surgical — improve wording, not meaning.
3. Verify no `\cite{}`/`\ref{}`/number changed; acronyms still defined on first use.
4. Run a **quality-reviewer** pass and address what it raises.

## Output
The improved section written back to its `.tex` file, plus a short list of the main changes
(e.g. "removed 3 filler phrases, unified 'HAL driver', added interpretation to Table~\ref{...}")
and anything flagged for the author.
