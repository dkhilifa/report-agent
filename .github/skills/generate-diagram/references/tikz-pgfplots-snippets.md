# TikZ / pgfplots / booktabs Snippet Library

Ready-to-edit **tables and numeric data plots** for the **generate-diagram** skill — the visuals
that stay native LaTeX so table text remains selectable and plot axes stay accurate. **Structural
diagrams** (architecture, workflow, block, flowchart, memory layout) are now authored in draw.io;
see [the draw.io template library](./drawio-templates.md). The TikZ diagrams in sections 5–7 below
are kept only as a no-tooling fallback for when draw.io is unavailable.

All are pdfLaTeX-safe and use packages already in `Packages.tex`, **except pgfplots** — for any
plot, first add to `Packages.tex`:

```latex
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
```

Replace `<...>` placeholders with real values. Keep one consistent visual style across the report.

---

## 1. Comparison table (booktabs)

```latex
\begin{table}[H]
  \centering
  \begin{tabular}{l r r r}
    \toprule
    Configuration & Cycles & Flash (KB) & RAM (KB) \\
    \midrule
    HAL          & 1\,234 & 12.4 & 3.2 \\
    LL           &   612  &  8.1 & 2.0 \\
    CMSIS only   &   540  &  6.7 & 1.8 \\
    \bottomrule
  \end{tabular}
  \caption{<Metric> by driver layer on <target> at <freq>\,MHz.}
  \label{tab:<key>}
\end{table}
```

---

## 2. Grouped bar chart (pgfplots)

Magnitude comparison across configurations (e.g. cycles per driver layer, two targets).

```latex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}
    \begin{axis}[
        ybar, bar width=10pt,
        width=0.85\textwidth, height=6cm,
        ymin=0, ylabel={Cycles},
        symbolic x coords={HAL, LL, CMSIS},
        xtick=data,
        legend pos=north east,
        nodes near coords, every node near coord/.append style={font=\footnotesize},
        enlarge x limits=0.25,
    ]
      \addplot coordinates {(HAL,1234) (LL,612) (CMSIS,540)}; % STM32F4
      \addplot coordinates {(HAL, 980) (LL,520) (CMSIS,470)}; % STM32H7
      \legend{STM32F4, STM32H7}
    \end{axis}
  \end{tikzpicture}
  \caption{Execution cost by driver layer on two targets (lower is better).}
  \label{fig:<key>}
\end{figure}
```

---

## 3. Line plot (pgfplots)

A metric versus a swept parameter (clock frequency, optimization level).

```latex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}
    \begin{axis}[
        width=0.85\textwidth, height=6cm,
        xlabel={Clock frequency (MHz)}, ylabel={CoreMark},
        grid=major, legend pos=north west, mark size=2pt,
    ]
      \addplot coordinates {(72,180) (168,420) (480,1200)};
      \addplot coordinates {(72,165) (168,395) (480,1130)};
      \legend{\texttt{-O3}, \texttt{-Os}}
    \end{axis}
  \end{tikzpicture}
  \caption{CoreMark vs clock frequency for two optimization levels.}
  \label{fig:<key>}
\end{figure}
```

---

## 4. Scatter / trade-off plot (pgfplots)

Two metrics against each other (flash size vs speed).

```latex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}
    \begin{axis}[
        width=0.8\textwidth, height=6cm,
        xlabel={Flash (KB)}, ylabel={Cycles},
        grid=major, only marks, mark size=2.5pt,
        nodes near coords*={\config}, % optional labels
        visualization depends on={value \thisrow{label}\as\config},
    ]
      \addplot table [meta=label] {
        x     y     label
        12.4  1234  HAL
        8.1   612   LL
        6.7   540   CMSIS
      };
    \end{axis}
  \end{tikzpicture}
  \caption{Size--speed trade-off across driver layers (bottom-left is better).}
  \label{fig:<key>}
\end{figure}
```

---

## 5. Architecture diagram (TikZ)

Layered structure: application ↔ STM32Cube layers ↔ hardware.

```latex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}[
      node distance=6mm,
      box/.style={draw, rounded corners, minimum width=6cm, minimum height=9mm, align=center},
      hw/.style ={draw, fill=black!5, minimum width=6cm, minimum height=9mm, align=center},
  ]
    \node[box] (app)   {Application code};
    \node[box, below=of app] (mw) {Middleware (FreeRTOS, USB, FatFS)};
    \node[box, below=of mw]  (hal){HAL / LL drivers};
    \node[box, below=of hal] (cmsis){CMSIS (Cortex-M core \& DSP)};
    \node[hw,  below=of cmsis](mcu){STM32 MCU hardware (peripherals, memory)};
    \draw[-stealth] (app)--(mw); \draw[-stealth] (mw)--(hal);
    \draw[-stealth] (hal)--(cmsis); \draw[-stealth] (cmsis)--(mcu);
  \end{tikzpicture}
  \caption{STM32Cube software stack exercised by the benchmark.}
  \label{fig:<key>}
\end{figure}
```

---

## 6. Workflow / process diagram (TikZ)

A measurement or build pipeline.

```latex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}[
      node distance=10mm and 14mm,
      step/.style={draw, rounded corners, minimum height=9mm, align=center, fill=black!3},
      >=stealth,
  ]
    \node[step] (cfg)  {Configure\\(CubeMX)};
    \node[step, right=of cfg] (build){Build\\(GCC \texttt{-O?})};
    \node[step, right=of build](flash){Flash\\(CubeProgrammer)};
    \node[step, right=of flash](meas) {Measure\\(DWT cycles)};
    \node[step, right=of meas] (log)  {Log \&\\analyze};
    \draw[->] (cfg)--(build); \draw[->] (build)--(flash);
    \draw[->] (flash)--(meas); \draw[->] (meas)--(log);
  \end{tikzpicture}
  \caption{Benchmark measurement workflow.}
  \label{fig:<key>}
\end{figure}
```

---

## 7. Memory layout (TikZ stacked bar)

Flash or RAM occupancy by component.

```latex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}[ybar stacked, font=\footnotesize]
    \fill[black!10] (0,0)    rectangle (1.4,3.2); \node at (0.7,1.6){.text};
    \fill[black!25] (0,3.2)  rectangle (1.4,4.0); \node at (0.7,3.6){.rodata};
    \fill[black!40] (0,4.0)  rectangle (1.4,4.6); \node at (0.7,4.3){.data};
    \draw (0,0) rectangle (1.4,4.6);
    \node[anchor=west] at (1.7,2.3){Flash usage: 4.6\,KB total};
  \end{tikzpicture}
  \caption{Flash footprint by section for the <config> build.}
  \label{fig:<key>}
\end{figure}
```

---

### Notes
- Keep axis labels, units, and legends explicit; a figure must be readable without the caption.
- Use thin spaces in numbers (`1\,234`) and before units (`480\,MHz`) for consistency with the prose.
- For real device photos or IDE screenshots, use `\includegraphics` with files under `Figures/<topic>/`.
