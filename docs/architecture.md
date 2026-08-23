# Architecture

## ADC Architecture

The design uses a SAR architecture consisting of a sample-and-hold stage, capacitor DAC, dynamic comparator, SAR controller, and digital output/calibration stage.

## 1. Sample and Hold

The sampling switch captures the input voltage before the conversion begins.

Key effects to investigate:

- Charge injection
- Clock feedthrough
- On-resistance
- Acquisition time
- Settling

**Simulation results:** TODO

## 2. Capacitor DAC

A binary-weighted or segmented capacitor DAC is used to generate the successive approximation voltages.

The choice between binary-weighted and segmented implementations will be based on the trade-off between:

- Matching
- Area
- Switching energy
- Settling
- Complexity

**Final architecture:** TODO

**Simulation results:** TODO

## 3. Dynamic Comparator

A regenerative dynamic latch is used to compare the sampled input against the DAC voltage.

Important characteristics:

- Input-referred offset
- Regeneration time
- Decision delay
- Noise
- Metastability

**Comparator topology:** TODO

**Simulation results:** TODO

## 4. SAR Logic

The SAR FSM performs one comparison per bit, beginning with the MSB and progressing toward the LSB.

The controller must ensure correct synchronization between DAC settling, comparator evaluation, and bit decisions.

**Implementation:** TODO

## 5. Calibration

The digital calibration stage compensates for systematic errors introduced by the analog blocks.

**Selected calibration method:** TODO

**Measured improvement:** TODO
