---
name: generate-diagram
description: "Produce a diagram, figure, or comparison table for the STM32Cube Benchmark PFE report from data or a description. Use when the author asks for a 'diagram', 'figure', 'chart', 'plot', 'table', 'architecture diagram', or 'workflow'. Structural diagrams are authored in draw.io (.drawio), exported to a cropped PDF/PNG image with the draw.io desktop CLI, and integrated with \\includegraphics; numeric data plots use pgfplots and exact-number tables use booktabs."
argument-hint: "The data or concept to visualize, and where it will be used."
---

# Generate a Diagram or Figure

Turn a concept or benchmark data into a clear, compile-ready visual. **draw.io is the default
engine for diagrams** — anything structural: architecture, workflow/pipeline, block/data-flow,
flowchart, memory layout. The flow is always the same three moves the author asked for:

1. **Create** an editable `.drawio` file (the source of truth).
2. **Save it as an image** — export a cropped **PDF** (vector) or **PNG** with the draw.io CLI.
3. **Integrate** it into the report with `\includegraphics`, captioned and labeled.

Two visual kinds stay native LaTeX because draw.io is the wrong tool for them: **exact-number
tables** (`booktabs` — text must stay selectable) and **numeric data plots** with measured values
on real axes (`pgfplots` — axes must be accurate). Everything diagrammatic goes through draw.io.

## When to use
- A section needs an architecture/workflow/block diagram, a flowchart, or a memory-layout figure.
- The author hands over a structure or process and wants it drawn.
- (Tables/data charts) the author hands over numbers for a comparison table or a results chart.

## Tooling on this machine (verified)
- **draw.io desktop CLI:** `C:\Program Files\draw.io\draw.io.exe` (v30.x). Headless export confirmed
  working. Install/upgrade with `winget install JGraph.Draw`.
- **Visual editor:** VS Code extension `hediet.vscode-drawio` — open any `.drawio` to edit it on a
  canvas inside the editor; changes are plain XML in the same file.
- **pdfLaTeX includes PDF and PNG natively** (the template already uses `\includegraphics`). Do
  **not** feed pdfLaTeX an SVG — that needs the `svg` package plus Inkscape, which is not set up.

## Choose the right visual
| To show… | Use | Source library |
|----------|-----|----------------|
| System/peripheral structure, layered stack | **draw.io** architecture diagram | drawio-templates |
| A measurement/build/benchmark process | **draw.io** workflow / pipeline | drawio-templates |
| Component / data-flow relationships | **draw.io** block diagram | drawio-templates |
| A decision or branching procedure | **draw.io** flowchart | drawio-templates |
| Flash/RAM layout (conceptual) | **draw.io** stacked blocks | drawio-templates |
| Exact numbers across a few configs | `booktabs` table | tikz-pgfplots-snippets |
| Magnitude comparison (cycles, KB, mA) | pgfplots **bar chart** | tikz-pgfplots-snippets |
| Metric vs a swept parameter (clock, `-O?`) | pgfplots **line plot** | tikz-pgfplots-snippets |
| Size-vs-speed style trade-off | pgfplots **scatter** | tikz-pgfplots-snippets |

- draw.io XML templates + export commands: [the draw.io template library](./references/drawio-templates.md).
- Tables and numeric plots: [the TikZ/pgfplots/booktabs library](./references/tikz-pgfplots-snippets.md).

You can run the steps directly, or delegate authoring to the **diagram-designer** and the
`\includegraphics` wiring to the **latex-engineer**.

## Procedure — a draw.io diagram
1. **Pick the form** from the table; state in one line why it fits.
2. **Create the `.drawio`** — instantiate a template from the library, replace placeholders with the
   **real** labels and structure (mark estimates; never fabricate numbers). Save it as the editable
   source of truth at `Figures/<topic>/<name>.drawio` (one sub-folder per topic).
3. **Save it as an image** — run the draw.io CLI **from the report root**
   (`INPT_PFE_Report_Template/`) so the relative paths resolve:
   ```powershell
   $drawio = "C:\Program Files\draw.io\draw.io.exe"
   # PDF — vector, sharpest in print (recommended for diagrams)
   & $drawio --export --format pdf --crop --no-sandbox `
       --output "Figures/<topic>/<name>.pdf" "Figures/<topic>/<name>.drawio"
   # PNG — raster image, 3x scale, transparent background (use if you want a literal image)
   & $drawio --export --format png --scale 3 --transparent --no-sandbox `
       --output "Figures/<topic>/<name>.png" "Figures/<topic>/<name>.drawio"
   ```
   No CLI available? Open the `.drawio` in VS Code (drawio extension) or the desktop app and use
   **File → Export as → PDF/PNG** with *Crop* enabled, saving into the same `Figures/<topic>/` folder.
4. **Integrate** — drop the figure into the target section, captioned and labeled:
   ```latex
   \begin{figure}[H]
     \centering
     \includegraphics[width=0.85\textwidth]{Figures/<topic>/<name>.pdf}
     \caption{<What it shows and the takeaway>.}
     \label{fig:<key>}
   \end{figure}
   ```
   Swap to the `.png` path if you exported a raster. Reference it from the prose with `\ref{fig:<key>}`.
5. **Keep source + export together** — commit both `<name>.drawio` and the exported `<name>.pdf`/`.png`
   so the figure stays editable and reproducible. To revise: edit the `.drawio`, re-run the export.
6. **Confirm it compiles** — run **validate-latex** (or `pdflatex` twice) and check the figure appears
   in the List of Figures with the right number.

## Quality bar
- The diagram answers a question at a glance; styling is restrained and reuses the template palette
  so every figure looks like part of one report.
- Labels and units are explicit; the caption says what it shows **and** the takeaway; the label is
  referenced from the text.
- Prefer the **vector PDF** export for diagrams (crisp at any zoom in print); use PNG only when a
  raster is acceptable. Always crop whitespace.
- The `.drawio` source is committed next to its export. No fabricated values.

## Output
- **Chosen visual + one-line rationale.**
- **The `.drawio` file** (created) and **the export command** run to save the image.
- **The `\includegraphics` figure block**, captioned and labeled, placed in the target section.
- **Any preamble note** (e.g. adding `pgfplots` if a numeric plot was used instead) and the
  `\ref{}` key the prose should cite.
