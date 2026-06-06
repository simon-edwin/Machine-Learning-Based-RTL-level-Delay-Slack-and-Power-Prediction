# Machine Learning Based RTL Performance Prediction

## Overview

Traditional VLSI design flows rely on repeated logic synthesis and static timing analysis (STA) to evaluate circuit performance metrics such as delay, timing slack, and power. These processes are computationally expensive and increase design turnaround time during iterative design exploration.

This project presents a Machine Learning (ML)-based framework for early prediction of circuit performance using structural features extracted from RTL designs. By leveraging synthesis-generated netlists and timing information, a Random Forest regression model is trained to predict delay and power, while timing slack is computed from the predicted delay and user-defined timing constraints.

The proposed framework enables rapid performance estimation without requiring repeated synthesis and timing analysis, making it suitable for early-stage VLSI design space exploration.

---

## Objectives

- Automate extraction of structural RTL features.
- Generate timing and power datasets using open-source EDA tools.
- Train machine learning models for performance prediction.
- Predict delay, power, and timing slack for unseen designs.
- Reduce dependency on repeated synthesis and STA iterations.

---

## Methodology

### 1. RTL Design Preparation

A diverse set of combinational circuits was developed with multiple bit-width configurations:

- Ripple Carry Adders (RCA)
- Carry Look-Ahead Adders (CLA)
- Multiplexers (MUX)
- Comparators
- Multipliers
- Arithmetic Logic Units (ALU)

Bit-widths considered:

```
4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 64
```

---

### 2. Logic Synthesis

RTL designs were synthesized using **Yosys** to generate flattened gate-level netlists.

Outputs generated:

- Gate-level netlists
- Structural connectivity information

---

### 3. Structural Feature Extraction

Automated Python scripts were used to extract timing-relevant structural features:

| Feature | Description |
|----------|-------------|
| GateCount | Total number of logic gates |
| WireCount | Number of interconnections |
| InputBits | Input bus width |
| OutputBits | Output bus width |
| AvgFanout | Average fanout |
| MaxFanout | Maximum fanout |
| LogicDepth | Longest combinational path depth |

---

### 4. Timing Analysis

Static Timing Analysis (STA) was performed using **OpenSTA**.

Generated labels:

- Critical Path Delay
- Arrival Time
- Timing Slack

Slack calculation:

```text
Slack = Required Time − Delay
```

---

### 5. Power Estimation

A structural power model was used to estimate switching-dominated dynamic power based on:

- Gate count
- Wire count
- Fanout characteristics

The generated power values were used as targets for machine learning training.

---

### 6. Machine Learning Model

A **Random Forest Regressor** was trained using the extracted structural features.

Predicted outputs:

- Delay
- Power

Timing slack is computed from the predicted delay.

---

## Dataset

The dataset consists of synthesized RTL designs from multiple architectures and bit-widths.

### Input Features

```text
GateCount
WireCount
InputBits
OutputBits
AvgFanout
MaxFanout
LogicDepth
```

### Target Variables

```text
Delay
Power
```

---

## Results

### Delay Prediction

- R² Score: 0.989
- Mean Absolute Error (MAE): 1.13 ns

### Power Prediction

- R² Score: 0.992
- Mean Absolute Error (MAE): 0.096

The results demonstrate a strong correlation between structural RTL features and circuit performance metrics.

---

## Example Prediction

```python
required_time = 10

Predicted Delay  : 7.84 ns
Predicted Slack  : 2.16 ns
Predicted Power  : 3.52
Timing Status    : MET
```

---

## Tools and Technologies

### Hardware Design

- Verilog HDL
- Yosys
- OpenSTA

### Machine Learning

- Python
- Pandas
- NumPy
- Scikit-Learn
- Random Forest Regression

### Development Environment

- Ubuntu Linux
- Google Colab
- Jupyter Notebook

---

## Applications

- Early timing estimation
- RTL-level design exploration
- Fast architecture comparison
- ML-assisted Electronic Design Automation (EDA)
- Performance-aware RTL optimization

---

## Future Work

- Incorporate interconnect parasitics and routing effects.
- Support process-voltage-temperature (PVT) variations.
- Explore Graph Neural Networks (GNNs) and boosting methods.
- Extend the framework to sequential circuits.
- Integrate into automated CAD workflows.

---

## Authors

Developed as part of an M.Tech project on Machine Learning Assisted VLSI Design Automation.

---
