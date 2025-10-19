# Week 4: RISC-V SOC Design and Tapeout Program

## Introduction to MOSFET Circuit Design and SPICE Analysis

This repository contains comprehensive documentation and analysis for Week 4 of the VSD RISC-V SOC Tapeout Program. The focus is on understanding fundamental circuit design principles, MOSFET device physics, and SPICE simulation techniques essential for modern IC design.

## Quick Navigation
| Day | Topic | Link |
|-----|-------|------|
| 1 | Fundamentals of Circuit Design and SPICE Simulation | [Day 1](Day1/README.md) |
| 2 | Advanced SPICE Analysis and Channel Length Effects | [Day 2](Day2/README.md) |
| 3 | VTC Simulation and Timing Analysis | [Day 3](Day3/README.md) |
| 4 | Noise Margin Analysis and Circuit Robustness | [Day 4](Day4/README.md) |
| 5 | Power Scaling and Process Variation Analysis | [Day 5](Day5/README.md) |

## Course Overview

### Core Learning Objectives
1. **MOSFET Device Physics**: Understanding NMOS/PMOS transistor operation, threshold voltage characteristics, and current-voltage relationships
2. **SPICE Simulation**: Mastering circuit simulation techniques for design verification and characterization
3. **CMOS Inverter Design**: Analyzing voltage transfer characteristics, timing parameters, and noise margins
4. **Design Optimization**: Power scaling, process variation analysis, and device sizing strategies

### Key Topics Covered

#### 1. NMOS Transistor Fundamentals
- Device structure and operation principles
- Threshold voltage calculation with body effect
- Current-voltage characteristics in different operating regions
- Long-channel vs short-channel device behavior

#### 2. SPICE Simulation Methodology  
- Circuit netlist creation and syntax
- Model parameter specification
- DC, AC, and transient analysis techniques
- Result interpretation and validation

#### 3. CMOS Inverter Characterization
- Voltage Transfer Characteristic (VTC) analysis
- Switching threshold (Vm) calculation and optimization
- Rise time and fall time extraction
- Device sizing impact on performance metrics

#### 4. Noise Margin and Robustness
- Digital logic level definitions (VIL, VIH, VOL, VOH)
- Noise margin calculation from VTC curves
- Robustness evaluation under process variations
- Design guidelines for reliable operation

#### 5. Power and Performance Optimization
- Supply voltage scaling effects on performance and power
- Process variation impact (etching, oxide thickness)
- Device sizing strategies for balanced performance
- Energy-delay trade-offs in digital circuits

## Repository Structure

```
├── Day1/          # Circuit Design Basics and NMOS Theory
├── Day2/          # SPICE Analysis and Channel Length Effects  
├── Day3/          # VTC Simulation and Timing Analysis
├── Day4/          # Noise Margins and Design Robustness
├── Day5/          # Power Scaling and Process Variations
└── README.md      # This overview document
```

## Daily Contents

### [📘 Day 1: Fundamentals of Circuit Design and SPICE Simulation](Day1/README.md)
- NMOS transistor architecture and operation principles
- Threshold voltage analysis and body effect
- Operating regions (cut-off, linear, saturation)
- Introduction to SPICE simulation and netlist structure
- Device physics and PN junction fundamentals

### [📊 Day 2: Advanced SPICE Analysis and Channel Length Effects](Day2/README.md)
- Long-channel vs short-channel MOSFET comparison
- Velocity saturation effects in modern devices
- Four regions of operation in short-channel devices
- CMOS inverter fundamentals and switch analysis
- Voltage Transfer Characteristics (VTC) derivation

### [⚡ Day 3: VTC Simulation and Timing Analysis](Day3/README.md)
- Complete CMOS inverter VTC analysis
- Switching threshold calculation and optimization
- Rise time and fall time characterization
- Device sizing impact on performance metrics
- Timing parameter extraction techniques

### [🛡️ Day 4: Noise Margin Analysis and Circuit Robustness](Day4/README.md)
- Digital logic level definitions (VIL, VIH, VOL, VOH)
- Noise margin calculation from VTC curves
- Process variation impact on noise margins
- Design guidelines for robust operation
- Reliability and manufacturability considerations

### [⚙️ Day 5: Power Scaling and Process Variation Analysis](Day5/README.md)
- Supply voltage scaling effects on performance
- Power consumption analysis and optimization
- Process variation modeling and impact
- Device sizing strategies for power-performance trade-offs
- Advanced design techniques for modern processes

## Prerequisites

- Basic understanding of semiconductor physics
- Familiarity with digital logic concepts
- SPICE simulation environment (ngspice recommended)
- SKY130 PDK for device models

## Getting Started

1. Clone this repository to your local workspace
2. Install ngspice simulation environment
3. Obtain SKY130 process design kit (PDK)
4. Follow the daily progression through Day1-Day5 directories
5. Run provided SPICE simulations to verify theoretical concepts

## Key Simulation Tools

- **ngspice**: Open-source SPICE simulator for circuit analysis
- **SKY130 PDK**: SkyWater 130nm process technology models
- **Plotting Tools**: For waveform analysis and characteristic extraction

## Learning Outcomes

Upon completion of this week's material, you will be able to:
- Design and simulate basic CMOS circuits using SPICE
- Analyze MOSFET device characteristics and optimize sizing
- Extract key performance metrics (delay, power, noise margins)
- Understand the impact of process variations on circuit behavior
- Apply design-for-manufacturability principles in IC design

## References and Resources

- SkyWater SKY130 Process Documentation
- ngspice User Manual and Simulation Examples
- CMOS VLSI Design Principles and Applications
- Process Variation and Design Robustness Literature

---

*This documentation is part of the VSD RISC-V SOC Tapeout Program Week 4 curriculum, focusing on fundamental circuit design and simulation skills essential for modern IC development.*