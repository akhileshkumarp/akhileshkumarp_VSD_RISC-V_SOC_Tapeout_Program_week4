# Day 3: VTC Simulation and Timing Analysis

## Overview
This document provides comprehensive coverage of CMOS inverter characterization through SPICE simulation. We explore voltage transfer characteristic (VTC) analysis, timing parameter extraction, switching threshold derivation, and the impact of transistor sizing on performance metrics.

## CMOS Inverter SPICE Simulation Methodology

### Circuit Setup and Node Identification

For accurate SPICE simulation of a CMOS inverter, proper circuit connectivity and node naming are essential:

**Circuit Components:**
- NMOS transistor (pull-down network)
- PMOS transistor (pull-up network)  
- Input voltage source
- Supply voltage source
- Load capacitance
- Proper node connections

**Node Naming Convention:**
- `vin`: Input node
- `vout`: Output node  
- `vdd`: Supply voltage node
- `0`: Ground reference node

### Complete SPICE Netlist for VTC Analysis

```spice
* CMOS Inverter VTC Analysis
* Circuit Description
Mn1 vout vin 0 0 nmos W=0.36u L=0.15u
Mp1 vout vin vdd vdd pmos W=1.08u L=0.15u
Cload vout 0 10f

* Voltage Sources
VDD vdd 0 1.8
Vin vin 0 0.9

* Include Technology Models
.include "sky130_fd_pr/models/nfet_01v8.spice"
.include "sky130_fd_pr/models/pfet_01v8.spice"

* DC Analysis for VTC
.dc Vin 0 1.8 0.01

* Control Commands
.control
run
plot vout vs vin
.endc

.end
```

### DC Analysis Configuration

**Parameter Settings:**
- **Start voltage**: 0V (logic low)
- **End voltage**: 1.8V (supply voltage)
- **Step size**: 0.01V (sufficient resolution for smooth curve)

**Expected Output**: VTC curve showing output voltage as function of input voltage

## Voltage Transfer Characteristic (VTC) Analysis

### VTC Curve Interpretation

The VTC curve reveals critical inverter characteristics:

1. **Logic High Output (VOH)**: Output voltage for low input
2. **Logic Low Output (VOL)**: Output voltage for high input  
3. **Switching Threshold (Vm)**: Input voltage where Vout = Vin
4. **Transition Region**: Region where output switches between logic levels
5. **Noise Margins**: Tolerance to input noise

### Key VTC Parameters

**VOH (Output High Voltage):**
- Ideally equals VDD
- Determines logic '1' level
- Affected by PMOS sizing and threshold voltage

**VOL (Output Low Voltage):**
- Ideally equals 0V
- Determines logic '0' level  
- Affected by NMOS sizing and threshold voltage

**Transition Width:**
- Determines switching sharpness
- Affects noise immunity
- Related to device gain in transition region

### Using SPICE Commands for VTC

```spice
.control
* Run DC analysis
run

* Plot VTC curve
plot vout vs vin

* Find switching threshold
let vm_line = vin
plot vout vs vin vm_line vs vin

* Measure specific points
meas dc voh find vout when vin=0
meas dc vol find vout when vin=1.8
.endc
```

## Transient Analysis and Timing Characterization

### Transient Simulation Setup

For timing analysis, we apply a time-varying input signal and observe the output response:

```spice
* CMOS Inverter Transient Analysis
* Same circuit as above, but with different sources

* Input Pulse
Vin vin 0 PULSE(0 1.8 1n 0.1n 0.1n 2n 5n)

* Transient Analysis
.tran 0.01n 10n

.control
run
plot vout vs time vin vs time
.endc
```

**Pulse Parameters:**
- **V1 = 0V**: Low voltage level
- **V2 = 1.8V**: High voltage level
- **TD = 1n**: Delay time  
- **TR = 0.1n**: Rise time
- **TF = 0.1n**: Fall time
- **PW = 2n**: Pulse width
- **PER = 5n**: Period

### Rise Time and Fall Time Extraction

**Definition of Timing Parameters:**

**Rise Time (tr)**: Time for output to transition from 10% to 90% of final value
**Fall Time (tf)**: Time for output to transition from 90% to 10% of initial value

**Alternative Definition (50% points):**
- **Propagation Delay Rise (tpdr)**: Time difference between 50% input and 50% output during rising transition
- **Propagation Delay Fall (tpdf)**: Time difference between 50% input and 50% output during falling transition

### SPICE Measurement Commands

```spice
.control
* Run transient analysis
run

* Measure rise time (10%-90%)
meas tran tr_10_90 trig vout val='0.1*1.8' rise=1 targ vout val='0.9*1.8' rise=1

* Measure fall time (90%-10%)  
meas tran tf_90_10 trig vout val='0.9*1.8' fall=1 targ vout val='0.1*1.8' fall=1

* Measure propagation delays (50% points)
meas tran tpdr trig vin val='0.9' rise=1 targ vout val='0.9' fall=1
meas tran tpdf trig vin val='0.9' fall=1 targ vout val='0.9' rise=1

* Calculate average propagation delay
let tpd_avg = (tpdr + tpdf)/2
print tpd_avg
.endc
```

### Manual Timing Measurement

For detailed analysis, extract time points manually:

**Step 1**: Identify 50% voltage levels
- **50% of VDD**: 0.9V for 1.8V supply

**Step 2**: Find time coordinates
- Input rising edge at 50%: t_in_rise
- Output falling edge at 50%: t_out_fall
- Input falling edge at 50%: t_in_fall  
- Output rising edge at 50%: t_out_rise

**Step 3**: Calculate delays
```
tpdr = t_out_fall - t_in_rise
tpdf = t_out_rise - t_in_fall
```

## Switching Threshold Voltage Analysis

### Definition and Significance

The switching threshold voltage (Vm) is the input voltage at which:
- **Vout = Vin**
- **Both transistors operate in saturation region**
- **Maximum current flows through the inverter**
- **Maximum power dissipation occurs**

Vm determines the noise margins and switching characteristics of the inverter.

### Graphical Determination

Plot the VTC curve with a 45° reference line (Vout = Vin):
- Intersection point gives Vm value
- Provides visual indication of switching balance

### Analytical Derivation of Vm

At the switching threshold, both transistors are in saturation with equal currents:

**Conditions at Vm:**
- VGSn = VGSp = Vm  
- VDSn = VDSp = Vm
- IDSn = -IDSp

**NMOS Current (saturation):**
```
IDSn = (1/2) μn Cox (Wn/Ln) (Vm - Vthn)²
```

**PMOS Current (saturation):**
```
IDSp = -(1/2) μp Cox (Wp/Lp) (VDD - Vm - |Vthp|)²
```

**Current Balance Equation:**
```
μn Cox (Wn/Ln) (Vm - Vthn)² = μp Cox (Wp/Lp) (VDD - Vm - |Vthp|)²
```

### Vm as Function of Device Ratios

Defining the ratio **r = (μp Cox (Wp/Lp)) / (μn Cox (Wn/Ln))**:

```
(Vm - Vthn)² = r (VDD - Vm - |Vthp|)²
```

Taking square root:
```
Vm - Vthn = √r (VDD - Vm - |Vthp|)
```

Solving for Vm:
```
Vm = (Vthn + √r (VDD - |Vthp|)) / (1 + √r)
```

### Special Case: Matched Inverter

For **r = 1** (equal drive strengths):
```
Vm = (Vthn + VDD - |Vthp|) / 2
```

If **Vthn = |Vthp| = Vth**:
```
Vm = VDD / 2
```

This represents a perfectly balanced inverter with maximum noise margins.

### Device Ratio as Function of Vm

Rearranging the switching threshold equation:
```
r = [(Vm - Vthn) / (VDD - Vm - |Vthp|)]²
```

This allows determination of required device sizing for target Vm.

## Impact of Transistor Sizing on Performance

### Systematic Sizing Study

We analyze the effect of PMOS sizing on inverter characteristics by varying Wp while keeping other parameters constant:

**Base Parameters:**
- **Ln = Lp = 0.15μm** (minimum length)
- **Wn = 0.36μm** (base NMOS width)
- **Wp = k × Wn** (variable PMOS width)

### Simulation Results Analysis

| Wp/Lp Ratio | Vm (V) | Rise Time (ns) | Fall Time (ns) | Comments |
|-------------|---------|----------------|----------------|----------|
| 2(Wn/Ln) | 0.86 | 0.372 | 0.283 | Fall time faster |
| 3(Wn/Ln) | 0.88 | 0.262 | 0.285 | Balanced timing |
| 4(Wn/Ln) | 0.89 | 0.202 | 0.287 | Rise time optimized |
| 5(Wn/Ln) | 0.91 | 0.167 | 0.338 | Excessive PMOS |

### Performance Trade-offs

**Rise Time Behavior:**
- Decreases with increasing PMOS size
- Due to stronger pull-up capability
- Limited by load capacitance and PMOS resistance

**Fall Time Behavior:**  
- Initially stable, then increases
- NMOS sizing becomes relatively weaker
- Increased junction capacitance affects speed

**Switching Threshold:**
- Increases with PMOS size
- Moves away from VDD/2 for large ratios
- Affects noise margins asymmetrically

### Optimal Sizing Strategy

**For Balanced Performance:**
- Choose **Wp/Lp ≈ 3(Wn/Ln)** for equal rise/fall times
- Provides good noise margins
- Reasonable power consumption

**For Speed-Critical Applications:**
- Larger PMOS for faster rise time
- Accept asymmetric timing if beneficial
- Consider increased power dissipation

**For Low-Power Applications:**
- Minimum sizing to meet timing requirements
- Accept longer delays for reduced power
- Balance static and dynamic power

### Design Guidelines

1. **Start with balanced sizing** (Wp/Lp = 2-3 × Wn/Ln)
2. **Verify timing requirements** for specific application
3. **Check noise margins** for robustness
4. **Consider process variations** in final sizing
5. **Validate with complete circuit simulations**

## Advanced Timing Considerations

### Load Capacitance Effects

**Capacitive Loading Sources:**
- Gate capacitance of driven devices
- Interconnect capacitance  
- Junction capacitance
- Parasitic capacitances

**Impact on Timing:**
```
τ = Ron × Cload
```

Where Ron is the effective on-resistance of the driving transistor.

### Process Variation Impact

**Critical Parameters:**
- Threshold voltage variations (±σVth)
- Mobility variations
- Oxide thickness variations
- Channel length variations

**Design Margins:**
- Include 3σ process variations
- Consider temperature effects (-40°C to 125°C)
- Account for supply voltage variations (±10%)

## Summary

This comprehensive analysis of CMOS inverter characterization demonstrates:

1. **VTC simulation** provides critical DC characteristics
2. **Transient analysis** reveals timing performance
3. **Switching threshold** determines noise immunity
4. **Device sizing** significantly impacts all performance metrics
5. **Trade-offs exist** between different performance objectives

The methodology presented enables systematic inverter design and optimization for specific application requirements.

## Key Takeaways

- **SPICE simulation** is essential for accurate characterization
- **VTC analysis** reveals switching behavior and noise margins
- **Timing extraction** requires careful measurement techniques
- **Switching threshold** can be analytically predicted and controlled
- **Device sizing** is the primary design knob for performance optimization
- **Balanced design** often provides the best overall performance

---

*Continue to Day 4 for noise margin analysis and robustness evaluation.*