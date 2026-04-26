# TPM-Based QKD Reconciliation Simulator

This repository contains the simulation code used in the research paper:

**S. Janavičiūtė, M. Niemiec, D. Razmadze, D. Drungilas, Ž. Lukošius, and M. Kurmis** *"Joint Parameter Scaling for Enhanced Security in Tree Parity Machine–Based QKD Reconciliation"* Klaipėda University, 2026.

---

## Overview

This project implements a **Tree Parity Machine (TPM)** simulator for Quantum Key Distribution (QKD) key reconciliation experiments. 

The simulator evaluates how jointly scaling the following parameters affects the reconciliation process:
* **$L$**: Synaptic depth (weight range)
* **$M$**: Input range

**Key metrics evaluated:**
* Synchronization speed between legitimate users.
* Passive attacker success rate.
* Attacker weight overlap.
* Extrema-value effects in final synchronized weights.

The implementation reproduces the Monte Carlo experiments detailed in the associated paper.

---

## Simulation Model

The simulation includes three TPMs:
1.  **Alice**
2.  **Bob**
3.  **Eve** (Passive attacker)

### Weight Initialization
Alice and Bob begin with highly correlated initial weights generated from bitstrings. Bob's weights are created by flipping bits in Alice's initial bitstring according to the **Quantum Bit Error Rate (QBER)**. Eve begins with a completely random weight vector.

### Attacker Model
Eve observes public inputs and public outputs. She only updates her internal state when:
* Alice and Bob’s outputs match.
* Eve’s own output matches theirs.

---

## Technical Details

### Learning Rule
The simulator utilizes **Hebbian learning**. Weights are clipped to the range:
$$[-L \times \text{scale}, L \times \text{scale}]$$

### Input Generation
The code supports **uniform inputs** (as used in the paper experiments). Random non-zero integers are sampled from:
$$\{-M, \dots, -1, 1, \dots, M\}$$

### Parameter Scaling
The simulation uses the formula:
$$L = L_0 \times 2^c$$

Where:
* $L_0 = 5$
* $c \in \{0, \dots, 11\}$
* $M \in \{1, 2, 3, 4, 5\} \times 2^c$

This approach preserves the $L/M$ ratio while expanding both parameters.

---

## Default Experimental Setup

Paper experiments were executed with the following configuration:
* **K**: 3
* **N**: 60
* **L**: 5
* **QBER**: 0.05
* **Runs**: 10,000 Monte Carlo runs per configuration

---

## Usage

### Running Experiments
Run the cells in the Jupyter notebook: `tpm_simulator_joint_L_M_simulator.ipynb`. You may use the predefined experimental values (located in the second cell) or define custom parameters.

### Output
Results are exported as CSV files named `results_q0.05_m{M}_l{L}_uniform.csv`. Each file contains:
* Synchronization steps
* Eve success flag & match percentage
* Initial weights (Alice, Bob, Eve)
* Final weights (Alice, Eve)

### Repository Structure
```text
├── tpm_simulator_joint_L_M_simulator.ipynb
├── results/
└── README.md
```
### Dependencies

    Python 3.x

    NumPy

Install requirements via pip:

```
pip install numpy
```

### Reproducibility & Citation

This repository is provided to reproduce paper results, validate findings, and explore improved TPM synchronization methods.

Note: If you use this repository in academic work, please cite the associated paper:

(Full citation to be added after formal publication)
