# Supportive-Tool Intake Checklist

What the **tool-analyst** must extract from a supportive tool before any writing begins.
A supportive tool is **software the author built themselves** to enhance the benchmarking effort;
it is documented as the author's own engineering contribution (problem, design, integration,
value), **not** for benchmark metrics. Pull every fact from the actual artifacts (code, config,
docs, sample outputs) — never guess.

## 1. Identity
- [ ] Tool name and one-line purpose
- [ ] Category (automation/orchestration, measurement harness, parser/aggregator, dashboard/reporting, CI/reproducibility, config generator)
- [ ] Confirm it is a *tool*, not a benchmark (if it produces measured MCU metrics, route to `benchmark-analyst`)

## 2. Problem it solves
- [ ] The benchmarking pain point or gap it addresses
- [ ] What it replaces (a manual step, an error-prone process, a missing capability)
- [ ] Why it was worth building

## 3. Capabilities
- [ ] What the tool actually does (concrete bullet list, grounded in the code)
- [ ] What it explicitly does **not** do (scope limits)

## 4. Architecture
- [ ] Main components / modules and their responsibilities
- [ ] Data and control flow: input → processing → output
- [ ] Any notable design pattern (pipeline, plugin, client/server, CLI + library)

## 5. Tech stack & dependencies
- [ ] Languages and frameworks (Python, Bash/PowerShell, C, Make/CMake, YAML…)
- [ ] Key libraries (e.g. `pyserial`, `pandas`, `matplotlib`, `click`/`argparse`)
- [ ] External tools it drives (STM32CubeProgrammer, STM32CubeCLT, OpenOCD, `arm-none-eabi-*`, IDE)
- [ ] Dependency manifest source (`requirements.txt`, `pyproject.toml`, `package.json`, `Makefile`)
- [ ] **Authorship boundary:** what the author built themselves vs the external libraries/tools reused (this is the citation boundary — own work is not cited, dependencies are)

## 6. Inputs & outputs
- [ ] Inputs consumed (files, formats, interfaces, device streams)
- [ ] Outputs produced (CSV/JSON, plots, logs, HTML/PDF reports, generated configs)
- [ ] Interfaces used (UART, SWO, ITM, DWT, OpenOCD, file system, network)

## 7. Integration with the benchmark workflow
- [ ] Where it plugs into the pipeline (configure → build → flash → measure → aggregate)
- [ ] Which benchmarks or steps depend on it
- [ ] How it ensures consistent conditions across runs/configurations

## 8. Usage
- [ ] How it is invoked (CLI command, GUI, CI step)
- [ ] A representative example invocation and its result
- [ ] Configuration/parameters that matter

## 9. Engineering value
- [ ] The concrete improvement (reproducibility, reliability, time saved, throughput, traceability)
- [ ] Evidence where available (e.g. unattended full sweeps, fewer manual errors)
- [ ] Known limitations and what remains manual

## 10. For the rest of the team
- [ ] Suggested visuals (architecture diagram, workflow/pipeline diagram, I/O table, config table)
- [ ] External facts/libraries that will need a citation (vendor tools, frameworks, techniques)
- [ ] Acronyms introduced
- [ ] Long code/config that belongs in the appendix (`Endmatter/ComplementaryCodes.tex`)
- [ ] Gaps / ambiguities to confirm with the author
