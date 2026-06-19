# Academic Style Guide — STM32Cube Benchmark PFE Report

Reference for the **improve-style** and **draft-section** skills and the **technical-writer**.
The goal: prose that reads like a competent engineer wrote it, suitable for a graded PFE jury.

## Voice and register
- **Engineering "we".** "We configured the timer…", "we observed a 2.1× speedup". Keep it
  consistent across every section. Do not drift into "I", "the author", or passive-only prose.
- **Confident but measured.** State findings plainly; hedge only where the data genuinely is
  uncertain ("the difference is within measurement noise"), not reflexively.
- **Academic, not stiff.** Precise and formal, yet readable. Contractions stay out; jargon is
  defined on first use.

## Sentence craft
- **Vary length and opening.** Mix short declaratives with longer analytical sentences. Avoid
  starting consecutive sentences the same way ("The benchmark… The benchmark… The benchmark…").
- **Connect ideas.** Use logical connectors to build an argument: *however, consequently, in
  contrast, as a result, therefore, whereas*.
- **Lead with the point.** The first sentence of a section states its substance — never restate
  the heading ("In this section, we will discuss…").

## Banned AI-tells and filler
Cut or replace on sight:
`delve`, `in the realm of`, `it is worth noting (that)`, `it is important to note`, `plays a
crucial/vital/pivotal role`, `tapestry`, `navigating the landscape/world of`, `a testament to`,
`unlock/unleash the power of`, `seamless(ly)`, `robust and scalable` (as filler), `at the end of
the day`, `when it comes to`, empty triads ("fast, efficient, and reliable" with no evidence),
and any sentence that adds words but no information.

## Marketing tone → evidence
Replace unverifiable superlatives with measured, sourced statements:
- "blazingly fast" → "completes in 612 cycles, 2.0× fewer than the HAL path".
- "the best option" → "the lowest-flash option among those tested, at 8.1 KB".

## Quantities and units
- Always attach units: cycles, µs/ms, KB (flash/RAM), mA/µA, µJ/mWh, MB/s, CoreMark/MHz, DMIPS/MHz.
- Use a thin space before units where the template allows (`480\,MHz`, `12.4\,KB`).
- Prefer exact figures over adjectives ("roughly halves" is fine *after* the number is given).

## Interpreting results (the part students skip)
Every table or figure needs prose that answers:
1. **What does it show?** (the headline comparison)
2. **Why?** (the mechanism: cache, driver overhead, optimization, DMA offload…)
3. **What is the trade-off?** (the winner *and its cost* in flash/RAM/power/portability)
4. **So what?** (the guidance for an engineer choosing a configuration)

Never end a results subsection on a raw number; end on its meaning.

## Terminology discipline
Pick one term per concept and keep it:
- "HAL driver" (not "Hal layer" / "abstraction lib"), "LL driver", "CMSIS".
- "optimization level" (not "opt setting"), "flash footprint", "static RAM".
- Expand each acronym on first use — "Direct Memory Access (DMA)" — then add it to
  `Endmatter/Acronyms.tex` and use the short form thereafter.

## Coherence with the whole report
- Match the depth and formatting of sibling sections.
- Cross-reference (`\ref{}`) instead of repeating material already written.
- When a new result changes the report's overall story, revise earlier framing and the conclusion.

## Quick before/after
> **Before:** "It is worth noting that the HAL plays a crucial role and is robust, delving into the
> realm of abstraction to deliver blazingly fast performance."
>
> **After:** "The HAL trades a little speed for portability: on the STM32H7 it took 1\,234 cycles
> against the LL path's 612, while keeping the same code across the F4 and H7 targets."
