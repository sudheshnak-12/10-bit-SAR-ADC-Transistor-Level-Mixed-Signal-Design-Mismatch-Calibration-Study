# ADC Specification

| Parameter | Specification |
|---|---|
| Resolution | 10 bit |
| Architecture | Single-ended SAR |
| Sampling rate | TODO |
| Supply voltage | TODO |
| Reference voltage | TODO |
| Technology / model | TODO |
| Target INL | Approximately ±1 LSB |
| Target DNL | Approximately ±1 LSB |
| Target pre-calibration ENOB | 7.5–9 bit |
| Target post-calibration ENOB | 9.5+ bit |
| Monte Carlo runs | 200+ where practical |

## Design Rationale

The 10-bit resolution provides enough sensitivity to make capacitor mismatch, comparator offset, and DAC settling meaningful while keeping the circuit practical for transistor-level simulation.

The remaining electrical specifications will be fixed before the final transistor-level implementation.

**Sampling rate:** TODO

**Supply:** TODO

**Reference:** TODO

**Technology/model:** TODO
