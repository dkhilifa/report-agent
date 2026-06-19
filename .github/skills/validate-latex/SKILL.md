---
name: validate-latex
description: "Validate the STM32Cube Benchmark PFE report's LaTeX for structure and compilation readiness. Use after editing .tex files, before declaring a section done, or when the author reports a build error. Checks balanced environments, escaped specials, resolvable \\ref/\\cite/\\label, figure paths, pdfLaTeX compatibility, and attempts a trial compile when a TeX toolchain is available."
argument-hint: "Optionally the file(s) to validate; default is the whole report."
---

# Validate LaTeX Structure and Compilation Readiness

Catch problems before they break the build. Delegate to the **latex-engineer**.

## When to use
- Right after writing or editing any `.tex` content.
- Before reporting a task complete, or when a compile fails.

## Structural checks (always)
1. **Balanced environments.** Every `\begin{env}` has a matching `\end{env}`
   (`figure`, `table`, `tabular`, `longtable`, `lstlisting`, `itemize`, `enumerate`, `algorithm`).
2. **Balanced braces** in commands; no stray `}` or missing `{`.
3. **Escaped specials** in text: `\% \& \_ \# \$`; backslash as `\textbackslash`.
4. **Resolvable references:**
   - every `\ref{k}` / `\eqref{k}` / `\autoref{k}` has a `\label{k}`;
   - every `\cite{k}` has a `\bibitem{k}` in `main.tex`;
   - no duplicate `\label{}` keys.
5. **Figures:** each `\includegraphics{path}` points to a real file under `Figures/`/`Logos/`;
   every `figure`/`table`/`lstlisting` has a `\caption{}` and a `\label{}`.
6. **pdfLaTeX compatibility:** no XeLaTeX/LuaLaTeX-only packages (`fontspec`, `unicode-math`);
   any newly used package (e.g. `pgfplots`) is imported in `Packages.tex`.
7. **Placeholders:** no leftover `Lorem ipsum` or template `ref1`/`ref2` stubs in finished sections;
   list any `% TODO` markers.
8. **Encoding:** non-ASCII handled by the template's `inputenc` (µ, é…) — keep usage consistent.

## Trial compile (when a TeX toolchain is available)
If `latexmk`/`pdflatex` exists, build from the report root and read the log:
```bash
# from the report root (folder containing main.tex)
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
# or: pdflatex -interaction=nonstopmode main.tex  (run twice for refs; +bibtex if migrated)
```
Scan the log for `! ` errors, `LaTeX Warning: Reference ... undefined`, `Citation ... undefined`,
and overfull/underfull boxes that signal layout problems. If no toolchain is present, rely on the
structural checks above and say the trial compile was skipped.

> Use a bounded run with `-halt-on-error`; do not launch unbounded or interactive TeX sessions.

## Output — Validation Report
- **Status:** pass / pass-with-warnings / fail.
- **Errors** (blocking) — exact location + cause + fix applied or required.
- **Undefined refs/cites** and **duplicate labels** — with locations.
- **Warnings** — unescaped specials, missing captions/labels, layout boxes, `% TODO` left.
- **Compile result** — clean, the specific failures, or "trial compile skipped (no toolchain)".
