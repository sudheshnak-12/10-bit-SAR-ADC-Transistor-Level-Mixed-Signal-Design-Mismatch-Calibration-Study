# Calibration Theory

## Motivation

Capacitor mismatch changes the effective DAC weights and introduces conversion errors. Comparator offset can shift the decision threshold and produce additional code-dependent errors.

Calibration is therefore used to estimate and compensate for these errors digitally.

## Foreground LMS Calibration

A foreground calibration sequence can apply known inputs, estimate DAC weight errors, and iteratively update digital correction coefficients.

The LMS update can be represented as:

```text
w[n+1] = w[n] + μ e[n] x[n]
```

where `w` represents the calibration coefficients, `e` is the measured error, `x` is the calibration regressor, and `μ` is the adaptation step size.

**Final calibration model and coefficients:** TODO

## Redundancy / Sub-Radix-2 Calibration

An alternative approach is to introduce redundancy into the SAR conversion so that individual comparator or DAC errors can be tolerated and corrected.

This approach will be considered as an extension if the initial calibration implementation permits it.

**Selected approach:** TODO

**Measured calibration improvement:** TODO
