# Quantum Control & Optimization Toolkit

This project provides a comprehensive, Colab-ready Python notebook for exploring hardware-aware Variational Quantum Eigensolver (VQE) and Quantum Approximate Optimization Algorithm (QAOA) performance, including noise robustness and resource budgeting. It is designed as a reproducible workflow for NASA-scale quantum-hardware studies.

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Prerequisites](#prerequisites)
4. [Installation](#installation)
5. [Running the Notebook](#running-the-notebook)
6. [Notebook Structure](#notebook-structure)

   * [Part 1: Pulse-Level VQE Optimization & Noise Sensitivity](#part-1-pulse-level-vqe-optimization--noise-sensitivity)
   * [Part 2: QAOA Depth Scaling Analysis](#part-2-qaoa-depth-scaling-analysis)
   * [Part 3: Resource Budget Estimation](#part-3-resource-budget-estimation)
7. [Results & Interpretation](#results--interpretation)
8. [Extending the Analysis](#extending-the-analysis)
9. [License](#license)

---

## Overview

Quantum algorithms hold promise for solving classically intractable problems. This project demonstrates:

* **Pulse-Level VQE**: Optimizes microwave-pulse controls for a two-level system to minimize ground-state energy error.
* **Noise Sensitivity**: Monte Carlo analysis of amplitude jitter to gauge robustness of the VQE sweet spot.
* **QAOA Depth Scaling**: Benchmarks approximation ratios on a 3-node Max-Cut as circuit depth increases.
* **Resource Budgeting**: Estimates total experiment time (shots × depth × pulse duration) to meet target accuracy.

All simulations run purely in Python (NumPy, SciPy, Matplotlib) and can be executed in Google Colab or any Jupyter environment.

---

## Features

* Time-domain Schrödinger integration for pulse-level VQE using `scipy.integrate.solve_ivp`
* Optimization of pulse amplitude and duration using `scipy.optimize.minimize`
* Monte Carlo noise analysis (10% amplitude jitter)
* QAOA cost and mixer Hamiltonians built with tensor products
* Exhaustive grid search for p=1 angle optimization
* Approximation ratio vs. depth curves for p = 1…5
* Interactive visualizations: error bar plots, depth-scaling plots, heatmaps
* Tabulated resource budgets displayed via Pandas DataFrame

---

## Prerequisites

* Python 3.7 or higher
* Required packages:

  * `numpy`
  * `scipy`
  * `matplotlib`
  * `pandas`

> No specialized quantum SDKs are required; this is a classical simulation.

---

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/your-org/quantum-analysis.git
   cd quantum-analysis
   ```

2. (Optional) Create and activate a virtual environment:

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install dependencies:

   ```bash
   pip install numpy scipy matplotlib pandas
   ```

---

## Running the Notebook

1. Open the notebook in Jupyter or Google Colab:

   * Jupyter: `jupyter notebook AdvancedQuantumAnalysis.ipynb`
   * Colab: Upload the notebook to Colab or use the provided Colab badge.

2. Execute cells sequentially. All code blocks are self-contained and require no additional setup.

---

## Notebook Structure

### Part 1: Pulse-Level VQE Optimization & Noise Sensitivity

* Defines Pauli matrices and Hamiltonians (`H0`, `H1`).
* Implements a Gaussian pulse shape and Schrödinger ODE solver.
* Uses `scipy.optimize.minimize` to find optimal pulse parameters (A, T).
* Sweeps pulse durations and performs Monte Carlo amplitude jitter sampling.
* Plots mean energy error ± standard deviation vs. pulse duration.

### Part 2: QAOA Depth Scaling Analysis

* Constructs cost Hamiltonian for a 3-node triangle Max-Cut and mixing Hamiltonian.
* Performs grid search over (γ, β) to find p=1 optimal angles.
* Simulates QAOA circuits for p = 1…5, computing approximation ratios.
* Plots ratio vs. depth, with a target ratio threshold.

### Part 3: Resource Budget Estimation

* Defines `shots` and uses optimized pulse duration as per-layer time.
* Calculates total experiment time for each QAOA depth.
* Presents a Pandas DataFrame summarizing depth, ratio, and time.

---

## Results & Interpretation

* **VQE Sweet Spot**: Optimal pulse at A ≈ 4.88, T ≈ 0.64 yields energy error ≈ 0.016. Noise analysis confirms minimal jitter sensitivity at this T.
* **QAOA Depth Trade-off**: Approximation ratio reaches ≈1.0 at p=1 for this small graph; deeper circuits show diminishing returns.
* **Resource Budget**: Depth 1 meets a target ratio ≥ 0.9 with minimal time—guiding experiment planning on real hardware.

---

## Extending the Analysis

1. **Noise Modeling**: Integrate Lindblad master-equation solvers (e.g., QuTiP) for amplitude/phase damping channels.
2. **Pulse Shapes**: Compare DRAG, square, and Blackman envelopes for VQE fidelity vs. decoherence.
3. **Larger Systems**: Scale VQE to 4–6 qubits (small molecules) and QAOA to 4–5 node graphs.
4. **Parameter Landscapes**: Generate p > 1 heatmaps by fixing earlier angles and sweeping later layers.
5. **Hardware Validation**: Deploy optimized pulses via a quantum backend API and compare simulated vs. measured results.

---

## License

This project is released under the MIT License. See [LICENSE](LICENSE) for details.
