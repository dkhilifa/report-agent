# draw.io Template Library

Complete `.drawio` (mxfile) XML you can drop into `Figures/<topic>/<name>.drawio`, relabel, and
export to a pdfLaTeX-ready **PDF** (vector) or **PNG** image. Every template uses the standard
draw.io palette so figures stay visually consistent across the report.

> **Workflow:** create the `.drawio` → export it as an image with the CLI below → embed with
> `\includegraphics`. Keep the `.drawio` source committed next to its export so it stays editable.

---

## Export commands (draw.io desktop CLI)

Run from the **report root** (`INPT_PFE_Report_Template/`) so the relative paths resolve. The
Windows binary is at `C:\Program Files\draw.io\draw.io.exe`; on macOS/Linux the command is `drawio`.

```powershell
$drawio = "C:\Program Files\draw.io\draw.io.exe"

# PDF — vector, recommended for diagrams (sharpest in print)
& $drawio --export --format pdf --crop --no-sandbox `
    --output "Figures/<topic>/<name>.pdf" "Figures/<topic>/<name>.drawio"

# PNG — raster image, 3x scale, transparent background
& $drawio --export --format png --scale 3 --transparent --no-sandbox `
    --output "Figures/<topic>/<name>.png" "Figures/<topic>/<name>.drawio"
```

Useful flags:

| Flag | Effect |
|------|--------|
| `--format pdf\|png\|svg\|jpg` | Output type. Use **pdf** for diagrams, **png** for a raster image. |
| `--crop` | Trim the page to the diagram (do this for figures). |
| `--scale 3` | Render at 3× (PNG sharpness). `--width N` / `--height N` fit to pixels instead. |
| `--transparent` | Transparent PNG background. |
| `--border 8` | Add an N-pixel margin around the crop. |
| `--page-index 0` | Export a specific page of a multi-page `.drawio`. |
| `--no-sandbox` | Required for the headless Electron export on Windows. |

**No CLI?** Open the `.drawio` in VS Code (`hediet.vscode-drawio`) or the desktop app →
**File → Export as → PDF/PNG**, tick *Crop*, save into `Figures/<topic>/`.

## LaTeX integration

```latex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.85\textwidth]{Figures/<topic>/<name>.pdf}
  \caption{<What it shows and the takeaway>.}
  \label{fig:<key>}
\end{figure}
```

pdfLaTeX embeds **PDF and PNG** directly — no extra package. (Avoid SVG: it needs the `svg`
package + Inkscape, which is not configured here.)

## Palette (reuse these pairs)

| Role | fillColor | strokeColor |
|------|-----------|-------------|
| Primary / app (blue)   | `#dae8fc` | `#6c8ebf` |
| Software layer (green) | `#d5e8d4` | `#82b366` |
| Core / CMSIS (yellow)  | `#fff2cc` | `#d6b656` |
| Optional / AI (orange) | `#ffe6cc` | `#d79b00` |
| Hardware / store (grey)| `#f5f5f5` | `#666666` |
| Accent / warning (red) | `#f8cecc` | `#b85450` |

In an mxCell `value`, write `&amp;` for `&` and `&#10;` for a line break (with `html=1`).

---

## 1. Layered architecture (vertical stack)

Application ↔ STM32Cube layers ↔ hardware, with an optional side box wired into a layer.

```xml
<mxfile host="app.diagrams.net">
  <diagram id="arch" name="Architecture">
    <mxGraphModel dx="900" dy="640" grid="0" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="850" pageHeight="1100" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <mxCell id="app" value="Application code" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=13;" vertex="1" parent="1"><mxGeometry x="120" y="80" width="240" height="48" as="geometry" /></mxCell>
        <mxCell id="mw" value="Middleware (FreeRTOS, USB, FatFS)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontSize=13;" vertex="1" parent="1"><mxGeometry x="120" y="156" width="240" height="48" as="geometry" /></mxCell>
        <mxCell id="hal" value="HAL / LL drivers" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontSize=13;" vertex="1" parent="1"><mxGeometry x="120" y="232" width="240" height="48" as="geometry" /></mxCell>
        <mxCell id="cmsis" value="CMSIS (Cortex-M core &amp; DSP)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;fontSize=13;" vertex="1" parent="1"><mxGeometry x="120" y="308" width="240" height="48" as="geometry" /></mxCell>
        <mxCell id="hw" value="STM32 MCU hardware (peripherals, memory)" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontSize=13;" vertex="1" parent="1"><mxGeometry x="120" y="384" width="240" height="48" as="geometry" /></mxCell>
        <mxCell id="side" value="Benchmark harness&#10;(DWT cycle counter)" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#ffe6cc;strokeColor=#d79b00;fontSize=12;" vertex="1" parent="1"><mxGeometry x="430" y="232" width="180" height="48" as="geometry" /></mxCell>
        <mxCell id="e1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="app" target="mw"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="e2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="mw" target="hal"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="e3" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="hal" target="cmsis"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="e4" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="cmsis" target="hw"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="e5" value="taps" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=open;dashed=1;fontSize=11;" edge="1" parent="1" source="side" target="hal"><mxGeometry relative="1" as="geometry" /></mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## 2. Workflow / pipeline (horizontal, with an optional branch)

A measurement or build pipeline. Solid = always; dashed = optional step.

```xml
<mxfile host="app.diagrams.net">
  <diagram id="flow" name="Workflow">
    <mxGraphModel dx="1100" dy="500" grid="0" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1100" pageHeight="480" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <mxCell id="s1" value="Configure&#10;(CubeMX)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=12;" vertex="1" parent="1"><mxGeometry x="40" y="120" width="120" height="56" as="geometry" /></mxCell>
        <mxCell id="s2" value="Build&#10;(GCC -O?)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=12;" vertex="1" parent="1"><mxGeometry x="210" y="120" width="120" height="56" as="geometry" /></mxCell>
        <mxCell id="s3" value="Flash&#10;(CubeProgrammer)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontSize=12;" vertex="1" parent="1"><mxGeometry x="380" y="120" width="120" height="56" as="geometry" /></mxCell>
        <mxCell id="s4" value="Measure&#10;(DWT cycles)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontSize=12;" vertex="1" parent="1"><mxGeometry x="550" y="120" width="120" height="56" as="geometry" /></mxCell>
        <mxCell id="s5" value="Log &amp; analyze" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;fontSize=12;" vertex="1" parent="1"><mxGeometry x="720" y="120" width="120" height="56" as="geometry" /></mxCell>
        <mxCell id="opt" value="AI summary&#10;(optional)" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#ffe6cc;strokeColor=#d79b00;fontSize=12;" vertex="1" parent="1"><mxGeometry x="720" y="240" width="120" height="56" as="geometry" /></mxCell>
        <mxCell id="f1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="s1" target="s2"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="f2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="s2" target="s3"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="f3" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="s3" target="s4"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="f4" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="s4" target="s5"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="f5" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=open;dashed=1;" edge="1" parent="1" source="s5" target="opt"><mxGeometry relative="1" as="geometry" /></mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## 3. Block / component diagram (core + store + optional side)

Data flowing through components, a persistent store (cylinder), and an optional off-path module.

```xml
<mxfile host="app.diagrams.net">
  <diagram id="block" name="Components">
    <mxGraphModel dx="1000" dy="640" grid="0" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="900" pageHeight="600" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <mxCell id="in" value="Ingestion&#10;(parse inputs)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1"><mxGeometry x="60" y="160" width="150" height="60" as="geometry" /></mxCell>
        <mxCell id="core" value="Comparison core&#10;(deterministic)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1"><mxGeometry x="300" y="150" width="170" height="80" as="geometry" /></mxCell>
        <mxCell id="out" value="Export&#10;(report, CSV, PDF)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1"><mxGeometry x="560" y="160" width="150" height="60" as="geometry" /></mxCell>
        <mxCell id="db" value="Knowledge base&#10;(SQLite)" style="shape=cylinder3;whiteSpace=wrap;html=1;boundedLbl=1;backgroundOutline=1;fillColor=#f5f5f5;strokeColor=#666666;" vertex="1" parent="1"><mxGeometry x="320" y="330" width="120" height="80" as="geometry" /></mxCell>
        <mxCell id="ai" value="Optional AI assist&#10;(LLM, off by default)" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#ffe6cc;strokeColor=#d79b00;" vertex="1" parent="1"><mxGeometry x="300" y="40" width="170" height="56" as="geometry" /></mxCell>
        <mxCell id="b1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="in" target="core"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="b2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="core" target="out"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="b3" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;startArrow=block;endArrow=block;startFill=1;endFill=1;" edge="1" parent="1" source="core" target="db"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="b4" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=open;dashed=1;" edge="1" parent="1" source="ai" target="core"><mxGeometry relative="1" as="geometry" /></mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## 4. Flowchart with a decision

Start → process → decision → two labeled branches → end.

```xml
<mxfile host="app.diagrams.net">
  <diagram id="fc" name="Flowchart">
    <mxGraphModel dx="800" dy="720" grid="0" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="700" pageHeight="760" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <mxCell id="start" value="Start" style="ellipse;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1"><mxGeometry x="280" y="40" width="120" height="50" as="geometry" /></mxCell>
        <mxCell id="p1" value="Run scenario" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1"><mxGeometry x="280" y="130" width="120" height="50" as="geometry" /></mxCell>
        <mxCell id="dec" value="Within&#10;tolerance?" style="rhombus;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1"><mxGeometry x="275" y="220" width="130" height="90" as="geometry" /></mxCell>
        <mxCell id="retry" value="Re-tune &amp; retry" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;" vertex="1" parent="1"><mxGeometry x="500" y="245" width="120" height="50" as="geometry" /></mxCell>
        <mxCell id="end" value="Record result" style="ellipse;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1"><mxGeometry x="280" y="360" width="120" height="50" as="geometry" /></mxCell>
        <mxCell id="c1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="start" target="p1"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="c2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="p1" target="dec"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="c3" value="yes" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;fontSize=11;" edge="1" parent="1" source="dec" target="end"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="c4" value="no" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;fontSize=11;" edge="1" parent="1" source="dec" target="retry"><mxGeometry relative="1" as="geometry" /></mxCell>
        <mxCell id="c5" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;" edge="1" parent="1" source="retry" target="p1"><mxGeometry relative="1" as="geometry" /></mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## 5. Memory layout (stacked blocks)

Flash sections stacked with no gaps; relabel sizes per build.

```xml
<mxfile host="app.diagrams.net">
  <diagram id="mem" name="Memory">
    <mxGraphModel dx="600" dy="600" grid="0" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="500" pageHeight="560" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <mxCell id="title" value="Flash footprint: 4.6 KB" style="text;html=1;align=center;fontStyle=1;fontSize=13;" vertex="1" parent="1"><mxGeometry x="120" y="40" width="200" height="24" as="geometry" /></mxCell>
        <mxCell id="text" value=".text  (code)  3.2 KB" style="whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=12;" vertex="1" parent="1"><mxGeometry x="140" y="80" width="160" height="160" as="geometry" /></mxCell>
        <mxCell id="rodata" value=".rodata  0.8 KB" style="whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontSize=12;" vertex="1" parent="1"><mxGeometry x="140" y="240" width="160" height="60" as="geometry" /></mxCell>
        <mxCell id="data" value=".data  0.6 KB" style="whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;fontSize=12;" vertex="1" parent="1"><mxGeometry x="140" y="300" width="160" height="48" as="geometry" /></mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## Style cheat-sheet (mxCell `style=` strings)

| Element | Style string |
|---------|--------------|
| Rounded box | `rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;` |
| Sharp box | `rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;` |
| Dashed (optional) | append `dashed=1;` |
| Ellipse (start/end) | `ellipse;whiteSpace=wrap;html=1;` |
| Decision diamond | `rhombus;whiteSpace=wrap;html=1;` |
| Database cylinder | `shape=cylinder3;whiteSpace=wrap;html=1;boundedLbl=1;backgroundOutline=1;` |
| Plain text label | `text;html=1;align=center;` |
| Solid arrow | `edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;endArrow=block;endFill=1;` |
| Dashed arrow | append `dashed=1;endArrow=open;` |
| Bidirectional | add `startArrow=block;startFill=1;` |

### Tips
- Keep `grid="0"` so exports are clean. `--crop` trims to content, so exact page size doesn't matter.
- One `.drawio` topic-folder per figure group (e.g. `Figures/footprint-tool/architecture.drawio`).
- Re-export after every edit; commit the `.drawio` and its `.pdf`/`.png` together.
- For a single round-trippable file you can also save as `<name>.drawio.svg` (editable + viewable),
  but still export a **PDF** for pdfLaTeX inclusion.
