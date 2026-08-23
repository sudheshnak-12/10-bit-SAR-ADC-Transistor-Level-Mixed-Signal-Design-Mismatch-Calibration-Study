# 10-bit SAR ADC: Transistor-Level Mixed-Signal Design, Mismatch & Calibration Study

## Overview

This project focuses on the design, simulation, and characterization of a 10-bit SAR ADC with particular emphasis on the non-idealities that determine real converter performance.

The ADC is built around a capacitor DAC, a dynamic latch comparator, and SAR control logic. The project goes beyond a functional ADC implementation by studying capacitor mismatch and comparator offset through Monte Carlo analysis and investigating a digital calibration scheme to compensate for these errors.

The main objective is to understand the complete mixed-signal design flow: architecture selection → transistor-level implementation → verification → statistical characterization → calibration.

## Target Roles

- Analog Design Engineer
- AMS / Mixed-Signal IC Design
- Data Converter Design
- Analog IP / IC Design

## Why SAR ADC?

SAR ADCs provide a useful intersection of analog and digital IC design. The DAC, sampling network, and comparator require transistor-level analog design, while the successive-approximation algorithm and calibration can be implemented digitally.

More importantly, practical SAR ADC performance is strongly affected by non-idealities such as capacitor mismatch, comparator offset, charge injection, and DAC settling. Studying these effects makes the project representative of the problems encountered in an actual mixed-signal design flow.

## Technical Objective

Design and simulate a 10-bit single-ended SAR ADC consisting of:

- Sample-and-hold / input sampling network
- Binary-weighted or segmented capacitor DAC
- Dynamic latch comparator
- SAR logic / finite-state machine
- Digital output register
- Digital calibration engine

The completed design will be characterized for:

- DNL and INL
- ENOB
- SNDR
- SFDR
- Comparator offset
- Capacitor mismatch sensitivity
- Monte Carlo performance distribution
- Calibration improvement

The initial architecture is single-ended, with a differential implementation as a possible extension.

## Architecture

```text
                    +------------------+
Vin -------------->|  Sample & Hold   |
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |     Cap-DAC      |<------+
                    | binary/segmented |       |
                    |  Vref switching  |       |
                    +--------+---------+       |
                             |                 |
                             v                 |
                    +------------------+       |
                    | Dynamic Latch    |       |
                    |   Comparator     |       |
                    +--------+---------+       |
                             |                 |
                       Decision Bit            |
                             |                 |
                             v                 |
                    +------------------+       |
                    |    SAR Logic     |-------+
                    |   bit-by-bit     |
                    |   approximation  |
                    +--------+---------+
                             |
                             v
                    +----------------------+
                    | Digital Output Reg.  |
                    | + Calibration Engine |
                    +----------------------+
```

The comparator output forms the main analog/digital boundary. The sampling network, capacitor DAC, switches, and comparator are treated as analog blocks, while SAR sequencing and calibration are handled digitally.

## Design Considerations

### Sample and Hold

The input sampling network will be studied for:

- MOS switch operation
- Charge injection
- Clock feedthrough
- Acquisition time
- Settling

### Capacitor DAC

The capacitor DAC will initially be implemented using a binary-weighted or segmented architecture.

The design will consider:

- Capacitor matching
- Unit-capacitor sizing
- Parasitic capacitance
- DAC settling
- Reference switching
- Area versus matching trade-offs

A segmented architecture may be used if it provides a meaningful improvement in matching or switching behavior.

### Dynamic Comparator

A dynamic latch comparator will be investigated for:

- Regeneration
- Input-referred offset
- Decision delay
- Noise
- Metastability
- Sensitivity to operating conditions

### SAR Logic

The SAR controller performs the binary search required for conversion.

The logic will be verified for:

- Correct bit-by-bit operation
- Conversion timing
- Comparator decision capture
- Proper DAC update sequence
- Output code generation
- Reset and start-of-conversion behavior

## Non-Idealities and Statistical Analysis

A major part of the project is understanding how practical circuit imperfections affect ADC performance.

Monte Carlo simulations will be used to study:

- Capacitor mismatch
- Comparator offset
- Resulting INL/DNL degradation
- ENOB degradation
- Statistical variation across multiple converter instances

The goal is not simply to obtain a nominally functioning ADC, but to quantify how robust the architecture is to realistic device variations.

## Calibration

A digital calibration scheme will be implemented to compensate for errors introduced by DAC mismatch and comparator imperfections.

Possible approaches include:

- Foreground LMS-based calibration
- Radix-based / redundancy calibration
- Sub-radix-2 error correction

The calibration stage will be evaluated using a before/after comparison of ADC performance.

The most important result will be the measured improvement in ENOB and linearity after calibration.

## Verification Methodology

### Static Characterization

A full-scale ramp or histogram test will be used to obtain the ADC transfer characteristic and extract:

- DNL
- INL
- Missing codes
- Monotonicity

### Dynamic Characterization

A coherent sine-wave input will be sampled and analyzed using an FFT.

The resulting spectrum will be used to determine:

- SNDR
- SFDR
- ENOB

The relationship

```text
ENOB = (SNDR - 1.76) / 6.02
```

will be used for ENOB extraction.

### Monte Carlo Characterization

Monte Carlo simulations will introduce variations in capacitor values and comparator offset.

The resulting distribution of ADC performance will be analyzed statistically.

A target of 200+ runs will be used where computationally practical.

### Corner Analysis

The comparator and DAC will also be evaluated across relevant:

- Process corners
- Supply variations
- Temperature variations

## Expected Performance

The following are design targets rather than measured results:

| Parameter | Target |
|---|---:|
| Resolution | 10 bit |
| DNL | Within approximately ±1 LSB |
| INL | Within approximately ±1 LSB |
| Pre-calibration ENOB | 7.5–9 bit |
| Post-calibration ENOB | Toward 9.5+ bit |
| Monte Carlo runs | 200+ where practical |

Actual performance will be determined from the completed simulations.

## Tools

- **ngspice / LTspice** — transistor-level circuit simulation
- **Xschem** — schematic capture
- **Python** — numerical analysis and automation
- **NumPy / SciPy / Matplotlib** — FFT and statistical analysis
- **Jupyter** — reproducible analysis
- **SkyWater SKY130 / predictive technology models** — transistor models where applicable

## Repository Structure

```text
sar-adc-mixed-signal/
│
├── README.md
│
├── docs/
│   ├── architecture.md
│   ├── spec.md
│   └── calibration_theory.md
│
├── spice/
│   ├── comparator/
│   ├── cap_dac/
│   ├── switches/
│   └── full_adc_tb.spice
│
├── sar_logic/
│
├── scripts/
│   ├── run_monte_carlo.py
│   ├── extract_inl_dnl.py
│   ├── fft_enob.py
│   └── calibration_lms.py
│
├── results/
│   ├── inl_dnl/
│   ├── fft_spectra/
│   └── monte_carlo/
│
├── figures/
│
└── notebooks/
```

## Implementation Plan

### Phase 1 — Specification

Fix:

- Resolution
- Sampling rate
- Supply voltage
- Reference voltage
- Technology / transistor model
- Target INL/DNL
- Target dynamic performance

The detailed specification is maintained in `docs/spec.md`.

### Phase 2 — Architecture

Select:

- Binary-weighted versus segmented capacitor DAC
- Dynamic comparator topology
- SAR FSM architecture

The architecture and design decisions are documented in `docs/architecture.md`.

### Phase 3 — Block-Level Implementation

Implement and simulate each block independently before integration.

The main checks are:

- Comparator offset and delay
- DAC settling
- Switch behavior
- Charge injection
- SAR timing

### Phase 4 — Full ADC Verification

Integrate the blocks and verify:

- Correct conversion
- Monotonicity
- Full-scale operation
- Correct SAR convergence
- Timing across all conversion cycles

### Phase 5 — Characterization

Extract:

- INL
- DNL
- ENOB
- SNDR
- SFDR

using dedicated static and dynamic testbenches.

### Phase 6 — Monte Carlo Analysis

Run statistical simulations for capacitor mismatch and comparator offset.

The resulting performance distribution will be compared with nominal performance.

### Phase 7 — Calibration

Implement the selected calibration method and repeat the characterization.

The final comparison will show:

```text
Before calibration → error / ENOB
After calibration  → error / ENOB
```

### Phase 8 — Final Analysis

Compare the results against the original specifications and analyze the main performance limitations.

## Results

The final results section will contain:

- ADC transfer characteristic
- INL/DNL plots
- FFT spectrum
- ENOB / SNDR / SFDR results
- Monte Carlo ENOB distribution
- Calibration convergence
- Before/after calibration comparison

Simulation-dependent values and plots are marked **TODO** until the corresponding simulations are completed.

## Limitations and Future Work

Potential extensions include:

- Fully differential implementation
- Layout-aware simulation
- Parasitic extraction
- Common-centroid capacitor layout
- Improved switching schemes
- Redundant / sub-radix-2 SAR architecture
- PVT characterization
- More advanced digital calibration
