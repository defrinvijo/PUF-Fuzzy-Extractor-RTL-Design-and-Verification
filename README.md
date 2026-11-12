# RTL Design and Verification of a Repetition-Code Fuzzy Extractor for PUF-Based Key Recovery

## 1. Project Overview

This project implements and verifies a **Repetition-Code Fuzzy Extractor** using SystemVerilog.

The design is intended for PUF-based hardware authentication in the AuthentiRx root-of-trust chip, which is designed for medicine package authentication.

A Physical Unclonable Function (PUF) generates a unique device-specific response using manufacturing variations. However, PUF responses may contain errors due to noise and environmental changes.

The fuzzy extractor uses repetition coding and majority voting to recover the original secret key from noisy PUF responses.

The design is implemented using RTL and functionally verified using **EDA Playground with Cadence Xcelium**.

## 2. Objectives

- Design a repetition-code fuzzy extractor using SystemVerilog.
- Recover secret keys from noisy PUF responses.
- Implement majority-voting-based error correction.
- Support streaming input data.
- Verify functional correctness using a self-checking testbench.
- Analyze key-recovery failure rates using Monte Carlo simulation.

## 3. System Architecture

The design uses the Code-Offset Construction.

### Enrollment Phase

1. Generate a random secret key (K).
2. Measure the raw PUF response (R).
3. Repeat each key bit REP times to form a codeword (C).
4. Generate public helper data:

   **W = R XOR C**

### Key Recovery Phase

1. Receive the noisy PUF response (R').
2. Combine it with the public helper data (W).
3. Perform XOR operation to recover the noisy codeword.
4. Apply majority voting to each group of REP bits.
5. Recover the original secret key.

## 4. Design Specifications

| Parameter | Value |
|---|---|
| Design Language | SystemVerilog |
| REP | 5 |
| KEY_BITS | 64 |
| NUM_PAIRS | 320 |
| Error Correction | Majority Voting |
| Simulator | Cadence Xcelium |
| Simulation Platform | EDA Playground |
| Waveform Viewer | EPWave |

## 5. RTL Module

**Module Name:** `rep_decoder`

### Input Signals

| Signal | Description |
|---|---|
| clk | System clock |
| rst_n | Active-low asynchronous reset |
| clear | Aborts a partially received key |
| wipe | Clears the recovered key |
| in_valid | Indicates valid input data |
| raw_bit | Raw PUF response bit |
| helper_bit | Public helper-data bit |

### Output Signals

| Signal | Description |
|---|---|
| key | Recovered 64-bit secret key |
| key_valid | Indicates successful key recovery |

## 6. Verification and Testing

A self-checking SystemVerilog testbench is used to verify the functionality of the RTL design.

The testbench performs five directed tests:

| Test | Description |
|---|---|
| Test 1 | Correctable noise within the majority-voting limit |
| Test 2 | Noise beyond the correction limit |
| Test 3 | Mid-key abort using the clear signal |
| Test 4 | Back-to-back key recovery |
| Test 5 | Key register wipe operation |

### Monte Carlo Simulation

Monte Carlo testing is performed to evaluate the key-recovery failure rate under different bit-error probabilities.

The simulation uses 400 trials for each error rate.

Tested bit-error rates:
- 2%
- 5%
- 10%
- 15%
- 20%

The repetition factor can also be changed to compare REP = 5, REP = 9 and REP = 15.

## 7. Tools and Technologies

- SystemVerilog
- RTL Design
- Functional Verification
- EDA Playground
- Cadence Xcelium
- EPWave
- Monte Carlo Simulation

## 8. Repository Structure

```text
PUF-Fuzzy-Extractor/
│
├── design.sv
│   └── RTL implementation
│
├── testbench.sv
│   └── Self-checking verification testbench
│
└── README.md
    └── Project documentation
```

## 9. Running the Simulation

1. Open [EDA Playground](https://www.edaplayground.com/).
2. Select SystemVerilog.
3. Copy `design.sv` into the Design pane.
4. Copy `testbench.sv` into the Testbench pane.
5. Select Cadence Xcelium as the simulator.
6. Enable Open EPWave after run.
7. Click Run to execute the simulation.
8. Observe the simulation output and waveforms.

## 10. Expected Results

The design is verified through five directed functional tests.

Expected verification results:

- Correct recovery under correctable noise.
- Incorrect decoding when the noise exceeds the correction limit.
- Successful recovery after a mid-key abort.
- Successful back-to-back key recovery.
- Successful key wiping.

The Monte Carlo experiment evaluates how the repetition factor affects the probability of recovering the complete 64-bit key.

## 11. Applications

- PUF-based hardware authentication.
- Secure semiconductor devices.
- Hardware root-of-trust systems.
- Anti-counterfeiting and medicine package authentication.
- Secure key generation and recovery.

## 12. Future Scope

- Integration with an actual Ring Oscillator PUF.
- Optimization of hardware area and power consumption.
- Implementation on FPGA hardware.
- Integration with an HMAC-SHA256 authentication engine.
- Exploration of higher repetition factors for improved noise tolerance.

## 13. Conclusion

This project demonstrates the RTL design and functional verification of a repetition-code fuzzy extractor for PUF-based key recovery.

The implementation uses streaming input processing and majority voting to reconstruct a secret key from noisy PUF responses.

Functional verification and Monte Carlo simulation help evaluate the reliability of the design under different noise conditions.

---

**Project Domain:** VLSI Design and Verification  
**Design Language:** SystemVerilog  
**Simulation Platform:** EDA Playground  
**Simulator:** Cadence Xcelium
