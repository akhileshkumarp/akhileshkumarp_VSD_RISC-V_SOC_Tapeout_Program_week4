# Day 4: Noise Margin Analysis and Circuit Robustness

## Overview
In this document you will get a brief introduction to what is noise margin and what are the applications of noise margin. We will also go through how to calculate noise margin from the VTC curve obtained from the SPICE simulation of a CMOS inverter and also compare the noise margins for variation of transistor size as done for rise time, fall time and switching threshold. 

# Noise Margin
Noise margin is another factor that counts for the robustness of a CMOS inverter. Noise margin for a CMOS inverter refers to the amount of noise voltage that a digital circuit can tolerate at its input without causing an incorrect output level or logic error. It quantifies the robustness of the inverter against electrical disturbances or variations, ensuring that the circuit operates correctly even in the presence of unwanted signals or noise.

# Deriving Noise Margins
## Working of an Inverter
We know that an inverter gives the compliment of whatever input is given. The ideal VTC waveform for an inverter is as shown below. 

| Input        | Output       | 
| ------------ | ------------ | 
| 0            | 1            | 
| 1            | 0            |  

## Noise Margins

![Ideal VTC](assets/ideal%20vtc.png)

The slope here is infinite, which makes it a very good device with no deffects. But a practical inverter does not behave as such. An intermediate inverter VTC is given below that gives us an idea of differnt voltage levels.

![Little bit practival VTC](assets/little%20practical%20.png)

In the above waveform, there are different voltage levels such as -
- VIL = Input low voltage
- VIH = Input high voltage
- VOH = Output high voltage 
- VOL = Output low voltage

It can also be deduced that:
- The range between 0-VIL is considered logic 0. Any value between this range will give an output between VOH and Vdd as logic 1.
- The range between VIH-Vdd is considered logic 1. Any value between this range will give an output between VOL and 0 as logic 0. 

Let's implement this on a more practical VTC waveform like the one given below. 

![Practical VTC](assets/Practival%20VTC.png)

It can be observed that the waveform is not very straight due to the non idealities of the inverter. The range VOH-Vdd is considered as logic 1. The range 0-VOL is considered as logic 0. 

Another point to be noted is that `VOH` lies between `VIH and Vdd`. This is so that the next stage of the input detectd logic 1 at the output of the invertor. A similar observation is made where `VOL` lies between `VIL and 0`. This is so that the next stage of the input detects logic 0 at the output of the invertor. Now to obtain the noise margins we plot all the voltage levels onto a single line as shown below:

### One Line Plot

![One Line Plot](assets/One%20line%20Plot.png)

From the plot we can observe the following - 
- NMH = VOH - VIH (Logic 1)
- NML = VIL - VOL (Logic 0)
- Undefined Region (Unkown logic state, can be either 0 or 1)

## Applications 
Noise margins are maily used for detecting bumps which can be further explained from the below image:

![Bump Detection](assets/Bump%20Detection%20.png)

From the above images, the following observations can be made:
- The first bump is between VOL-VIL, so it represents logic 0.
- The second bump is in the uncertain region meaning it can take values of either 0 or 1. 
- The third bump is between VIH-VOH, so it represents logic 1

Thus the conclusion that the first and third bumps are valid can be made. 

# Determining Noise Margin 
In this section we are going to look at how to measure the nosie margin of a CMOS inverter from the SPICE simulation output. The SPICE deck used for this experiment is given below:

![SPICE Deck](assets/spice%20deck.png)

The output recived upon simulation is given below:

![VTC curve](assets/vtc%20curve.png)

To find the noise margins we simple click at the points where `slope = -1` and perform calculations on the obtained co-ordinate values. 

![VTC Noise Margin](assets/vtc%20noice%20margin.png)

### Voltage Co-ordinates
![Output Co-ordinates](assets/noise%20margin.png)
From the above voltage points, the following outputs are obtained:
- VIL = 0.78351 V
- VIH = 0.96593 V
- VOH = 1.68718 V
- VOL = 0.13333 V
- NMH = VOH - VIH = 0.72125 V
- NML = VIL - VOL = 0.65018 V

# Noise Marging Comparision
With reference to the previous section, we are going to compare the noise margins for devices with respect to Wp/Lp = x(Wp/Lp) with a starting value of Wn = 0.36u and Ln = 0.15u. Upon following the steps from above, the below obseravtions were made:

| Wp/Lp        | Wn/Ln        | NMH (V)             | NML (V)        | 
| ------------ | ------------ | ------------------- | -------------- | 
| Wp/Lp        | 2(Wn/Ln )    | 0.736268            | 0.641758       | 
| Wp/Lp        | 3(Wn/Ln )    | 0.725275            | 0.672528       | 
| Wp/Lp        | 4(Wn/Ln )    | 0.702562            | 0.693407       | 
| Wp/Lp        | 5(Wn/Ln )    | 0.692670            | 0.697802       | 

# Conclusion 
In conclusion, we can tell:
- When the CMOS inverter operates in NML and NMH it can be used for digital design.
- When the CMOS inverter operates in the uncertain NM, it can be used for analog design.

**Overall Noise Margin:**
```
NM = min(NMH, NML)
```

The circuit can tolerate noise up to the minimum of these two values while maintaining correct logic operation.

## Theoretical Foundation: Ideal vs. Practical VTC

### Ideal Inverter Characteristics

An ideal CMOS inverter would exhibit:
- **Infinite gain** in the transition region
- **Zero transition width**
- **VOH = VDD** and **VOL = 0V**
- **Perfect noise immunity**

**Ideal VTC:**
```
Vout = VDD  for  Vin < VDD/2
Vout = 0    for  Vin > VDD/2
```

### Practical Inverter Behavior

Real inverters deviate from ideal behavior due to:
- **Finite transistor gain**
- **Non-zero output resistance**
- **Threshold voltage variations**
- **Channel length modulation**

The practical VTC exhibits:
- **Gradual transition** between logic levels
- **Finite slope** in switching region
- **Non-ideal output levels**

### Relationship Between VTC and Logic Levels

The logic levels are derived from VTC characteristics:

**VOH and VOL**: Direct measurements from VTC curve
- VOH = Vout when Vin = 0V
- VOL = Vout when Vin = VDD

**VIH and VIL**: Points where dVout/dVin = -1
- These points define the boundaries of the undefined region
- Maximum acceptable slope for logic level definition

## SPICE-Based Noise Margin Extraction

### Simulation Setup for Noise Margin Analysis

```spice
* CMOS Inverter Noise Margin Analysis
* Device Definitions
Mn1 vout vin 0 0 nmos W=0.36u L=0.15u
Mp1 vout vin vdd vdd pmos W=1.08u L=0.15u

* Load Capacitance
Cload vout 0 10f

* Voltage Sources
VDD vdd 0 1.8
Vin vin 0 0.9

* Include Models
.include "sky130_models.spice"

* DC Sweep for VTC
.dc Vin 0 1.8 0.001

* Derivative Calculation for Slope
.control
run
let vout_deriv = deriv(vout)
plot vout vs vin
plot vout_deriv vs vin
.endc

.end
```

### Identifying Critical Points

**Method 1: Graphical Analysis**

1. **Plot VTC curve**: Vout vs Vin
2. **Add unity slope line**: Vout = Vin for reference
3. **Locate intersection**: Gives switching threshold Vm
4. **Find slope = -1 points**: These define VIL and VIH

**Method 2: Analytical Extraction**

```spice
.control
* Run simulation
run

* Calculate derivative
let slope = deriv(vout)

* Find points where slope = -1
meas dc vil find vin when slope=-1 cross=1
meas dc vih find vin when slope=-1 cross=last

* Measure output levels
meas dc voh find vout when vin=0
meas dc vol find vout when vin=1.8

* Calculate noise margins
let nmh = voh - vih
let nml = vil - vol

print nmh nml
.endc
```

### Manual Measurement Procedure

**Step 1: Generate VTC Data**
- Perform DC sweep with fine resolution (≤1mV steps)
- Export data to analysis tool
- Calculate numerical derivative

**Step 2: Locate Critical Points**
- Find maximum and minimum slopes
- Identify crossings of slope = -1 threshold
- Extract corresponding voltage coordinates

**Step 3: Extract Voltage Levels**
```
VOH = Vout(Vin = 0V)
VOL = Vout(Vin = VDD)
VIL = Vin where dVout/dVin = -1 (first crossing)
VIH = Vin where dVout/dVin = -1 (last crossing)
```

**Step 4: Calculate Noise Margins**
```
NMH = VOH - VIH
NML = VIL - VOL
```

## Example: Detailed Noise Margin Analysis

### Simulation Results

For a representative CMOS inverter with VDD = 1.8V:

**Extracted Voltage Levels:**
- **VOH = 1.687V**: High output level  
- **VOL = 0.133V**: Low output level
- **VIH = 0.966V**: High input threshold
- **VIL = 0.784V**: Low input threshold

**Calculated Noise Margins:**
- **NMH = VOH - VIH = 1.687 - 0.966 = 0.721V**
- **NML = VIL - VOL = 0.784 - 0.133 = 0.651V**

### Interpretation of Results

**High Noise Margin (0.721V):**
- Input voltages above 0.966V are guaranteed to produce logic low output
- System can tolerate up to 721mV of positive noise on high inputs
- Represents 40% of supply voltage (good robustness)

**Low Noise Margin (0.651V):**
- Input voltages below 0.784V are guaranteed to produce logic high output  
- System can tolerate up to 651mV of negative noise on low inputs
- Represents 36% of supply voltage (adequate robustness)

**Overall Assessment:**
- Minimum noise margin = 651mV (36% of VDD)
- Well-balanced between high and low noise margins
- Provides good immunity to common noise sources

## Impact of Device Sizing on Noise Margins

### Systematic Sizing Study

Analysis of noise margins vs. PMOS sizing (keeping NMOS constant):

| Wp/Lp Ratio | NMH (V) | NML (V) | Total NM (V) | Balance |
|-------------|---------|---------|--------------|---------|
| 2(Wn/Ln) | 0.736 | 0.642 | 1.378 | Slight NMH bias |
| 3(Wn/Ln) | 0.725 | 0.673 | 1.398 | Well balanced |
| 4(Wn/Ln) | 0.703 | 0.693 | 1.396 | Nearly equal |
| 5(Wn/Ln) | 0.693 | 0.698 | 1.391 | NML slightly higher |

### Design Insights

**Optimal Sizing (Wp/Lp = 3-4 × Wn/Ln):**
- Provides balanced noise margins
- Maximizes minimum noise margin
- Good trade-off with other performance metrics

**Under-sized PMOS (Wp/Lp = 2 × Wn/Ln):**
- Higher NMH due to stronger NMOS relative to PMOS
- Lower NML due to weaker PMOS pull-up
- May compromise speed performance

**Over-sized PMOS (Wp/Lp = 5 × Wn/Ln):**
- Higher NML due to stronger PMOS
- Lower NMH due to weaker relative NMOS
- Increased area and capacitance

## Noise Margin Applications

### Signal Integrity Assessment

**Bump Detection and Classification:**

Noise margins enable quantitative assessment of signal integrity:

1. **Valid Logic 0**: Noise amplitude < NML
2. **Valid Logic 1**: Noise amplitude < NMH  
3. **Uncertain Region**: VIL < signal < VIH
4. **Invalid Signal**: Excessive noise causing logic errors

**Example Analysis:**
- Signal with 0.5V noise on logic 0 input
- Compare with NML = 0.651V
- Conclusion: Signal remains valid (0.5V < 0.651V)

### Design Margin Verification

**Process-Voltage-Temperature (PVT) Analysis:**

Noise margins must be verified across all operating conditions:

**Process Corners:**
- **Fast-Fast (FF)**: Best case performance
- **Slow-Slow (SS)**: Worst case performance  
- **Fast-Slow (FS)**: Skewed characteristics
- **Slow-Fast (SF)**: Skewed characteristics

**Temperature Range:**
- Commercial: 0°C to 70°C
- Industrial: -40°C to 85°C
- Automotive: -40°C to 125°C

**Supply Voltage:**
- Nominal ± tolerance (typically ±5% to ±10%)

### Interface Design

**Level Shifter Requirements:**

When interfacing circuits with different supply voltages:

```
VDD1 = 1.8V, VDD2 = 3.3V
```

**Compatibility Check:**
- VOH1 must exceed VIH2 for proper operation
- VOL1 must be below VIL2 for proper operation
- If not compatible, level shifters are required

## Advanced Noise Margin Considerations

### Dynamic Noise Margins

Static noise margins don't account for:
- **Switching noise**: Simultaneous switching effects
- **Supply bounce**: Dynamic supply variations
- **Ground bounce**: Ground potential variations
- **Crosstalk**: Coupling between signal lines

**Dynamic Noise Analysis:**
```spice
* Transient analysis with noise sources
Vnoise vin_noisy vin SIN(0 0.1 1MEG)
.tran 0.1n 100n
```

### Worst-Case Design Guidelines

**Conservative Design Rules:**

1. **Target noise margins ≥ 30% of VDD**
2. **Verify across all PVT corners**
3. **Include dynamic noise effects**
4. **Consider aging and reliability margins**
5. **Account for package and PCB parasitics**

### Technology Scaling Impact

**Noise Margin Scaling Challenges:**

As technology scales:
- **Supply voltage reduces**: VDD decreases faster than noise
- **Threshold voltage variation increases**: Relative variation grows
- **Noise sources increase**: Higher density, faster switching

**Mitigation Strategies:**
- **Careful device sizing**: Optimize for noise margins
- **Supply voltage regulation**: Tight voltage control
- **Noise-aware design**: Consider noise in all design phases
- **Process optimization**: Reduce threshold voltage variation

## Design-for-Test and Noise Margin Verification

### Built-in Self-Test (BIST) for Noise Margins

**On-Chip Noise Margin Monitor:**
```spice
* Simple noise margin test circuit
* Apply controlled noise and observe output
Vtest vin_test 0 PULSE(VIL-ΔV VIH+ΔV...)
```

**Measurement Strategy:**
- Gradually increase noise amplitude
- Monitor output for first logic error
- Extract actual noise margin in silicon

### Production Test Considerations

**Functional Test Vectors:**
- Include worst-case noise scenarios
- Test at multiple supply voltages
- Verify at temperature extremes

**Parametric Tests:**
- Direct measurement of VIL, VIH, VOL, VOH
- Comparison with design specifications
- Statistical analysis for process control

## Summary

Noise margin analysis is fundamental to robust digital design:

1. **Quantitative robustness assessment** through NMH and NML calculations
2. **SPICE-based extraction** enables accurate characterization
3. **Device sizing optimization** balances noise margins with other metrics
4. **Practical applications** include signal integrity and interface design
5. **Advanced considerations** address dynamic effects and scaling challenges

Proper noise margin design ensures reliable operation across all intended operating conditions and environments.

## Key Takeaways

- **Noise margins quantify circuit robustness** against various disturbances
- **SPICE simulation provides accurate extraction** of noise margin parameters
- **Device sizing significantly impacts** noise margin balance and magnitude
- **PVT analysis is essential** for robust design verification
- **Technology scaling challenges** require careful noise margin management
- **Design margins should account for** both static and dynamic noise effects

---

*Continue to Day 5 for power scaling analysis and process variation effects.*