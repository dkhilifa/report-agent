# Benchmark Intake Checklist

What the **benchmark-analyst** must extract from a subproject before any writing begins.
Pull every value from the actual artifacts (code, config, build output, logs) — never guess.

## 1. Identity
- [ ] Benchmark name and one-line purpose
- [ ] The question it answers (what decision does the result inform?)
- [ ] **Which competing vendor** ST is compared against (GD32, TI, NXP, Renesas, …) — ST is
      always the reference; the comparison is ST vs this one vendor
- [ ] **Which software stack** is under test (USB device stack, FreeRTOS, filesystem, TCP/IP, …)
- [ ] Which STM32Cube ecosystem aspect it exercises (CubeMX codegen, HAL vs LL, a middleware, a peripheral, the toolchain…)

## 2. What is measured
- [ ] Primary metric(s) and **units**: cycles, µs/ms, KB flash, KB RAM, mA, µJ/mWh, MB/s, CoreMark/MHz, DMIPS/MHz
- [ ] Whether higher or lower is "better"

## 3. Vendors & boards compared
Capture **both** platforms symmetrically — the ST reference and the competitor.
- [ ] ST board (NUCLEO/Discovery/Eval/custom) and MCU part number; competitor board and part number
- [ ] Core (Cortex-M0/0+/3/4/7/33… or vendor core) and clock frequency, for each
- [ ] Flash / RAM sizes, caches/accelerators (e.g. ART), for each
- [ ] Key peripherals relevant to the stack (e.g. USB FS/HS, DMA), for each
- [ ] Reference price / availability, for each (for the hardware-platforms table)

## 4. Stack & scenarios
- [ ] The stack under test and the list of **scenarios** that exercise it
      (e.g. USB → HID, CDC, MSC; FreeRTOS → context-switch, queue-throughput, interrupt-latency)
- [ ] For each scenario: what it stresses, the metric it yields, and **why it was chosen**
      (design rationale — representative use cases / device classes)
- [ ] Confirmation that every scenario is implemented **identically** on both platforms

## 5. Toolchain & build (per platform)
- [ ] IDE/compiler (STM32CubeIDE / GCC arm-none-eabi on the ST side; the vendor's IDE/GCC on the other) + version
- [ ] Optimization level (`-O0/-O1/-O2/-O3/-Os`) and notable flags
- [ ] STM32Cube firmware / HAL / LL / CMSIS versions and the vendor's equivalent stack versions
- [ ] Where size comes from (`arm-none-eabi-size`, `.map`, IDE build analyzer)

## 6. Methodology
- [ ] How the measurement is taken (DWT cycle counter, SysTick, timer, external power analyzer, logic analyzer)
- [ ] Sample count, warm-up, averaging, discarded outliers
- [ ] Fixed conditions (clock, voltage, temperature, compiler) held constant across both platforms
- [ ] Any methodology differences between scenarios or platforms (flag them)

## 7. Raw results
- [ ] The actual numbers, **per scenario and per platform** (ST vs competitor), with units (clean small tables)
- [ ] Any variance/error bars reported

## 8. Meaning
- [ ] The key finding in one sentence (where ST leads or trails the competitor, and by how much)
- [ ] The **trade-off** the data exposes (winner *and its cost*)
- [ ] Any surprise or anomaly worth flagging

## 9. For the rest of the team
- [ ] Suggested visuals (board-characteristics table? per-scenario results table? grouped bar chart? architecture/workflow diagram?)
- [ ] External facts that will need a citation (datasheet/board-spec figures, standard definitions, vendor specs)
- [ ] Acronyms introduced
- [ ] Gaps / ambiguities to confirm with the author
