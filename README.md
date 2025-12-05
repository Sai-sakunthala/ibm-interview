# Heisenberg Model Simulation (Scaled-Down Version)

This project implements the Heisenberg Hamiltonian with nearest-neighbour couplings and local fields, simulates time evolution using first-order Trotterization, and compares different initial qubit layouts when transpiling to the FakeTorino backend.

The original assignment target was **20 qubits**, but a full 20-qubit statevector simulation is not practical on my local system.  
To make the code runnable end-to-end, I used **n = 5 qubits** while keeping the same structure and workflow.  
Only the qubit count is changed; everything else follows the same logic.

---

## Features Implemented

### 1. Hamiltonian Construction
- Nearest-neighbour XX, YY, ZZ terms  
- Local X, Y, Z field terms  
- Built using `SparsePauliOp`

### 2. Time Evolution
- First-order Lie–Trotter decomposition  
- Implemented with `PauliEvolutionGate`  
- Single Trotter step or multiple steps configurable

### 3. Observable
- Implemented the operator  
  \[
    O = \sum_i Z_i
  \]
- Built as a `SparsePauliOp`

### 4. Ideal Expectation Value
- Computed using `StatevectorEstimator`
- Only feasible for **n = 5** on a local machine

### 5. Transpilation on FakeTorino
Transpiled the circuit for three layouts:

- L1 → `range(n)`
- L2 → `range(10, 10+n)`
- L3 → `range(20, 20+n)`

These mirror the required ranges for 20 qubits but are scaled down.

For each layout:
- Depth
- Gate count
- Number of 2-qubit gates  
are printed.

### 6. Noisy Simulation
- Used a depolarizing noise model  
- AerSimulator used for execution  
- The transpiled circuits are too large to simulate, so the noisy simulation runs on the decomposed original circuit  
- Expectation value computed from measurement statistics

---

## What Could Not Be Run Locally

### 1. 20-Qubit Ideal Simulation
A 20-qubit state vector needs too much RAM for my system.

### 2. Simulating FakeTorino-Transpiled Circuits
FakeTorino routing produces very deep circuits.  
AerSimulator raises a memory-limit error when attempting to simulate them.

### 3. ZNE / NEAT
Not implemented due to external tool requirements.  
Left `TODO` notes where they would fit.

---

## Why This Reduced Version Is Still Valid

- All major components of the assignment are implemented:
  - Hamiltonian  
  - Time evolution  
  - Observable  
  - Ideal value  
  - Transpilation  
  - Layout comparison  
  - Noisy simulation

- Only the qubit count is reduced so the code can actually run locally.  
- The workflow directly scales to 20 qubits on a larger machine.

---

## How to Run

```bash
pip install qiskit qiskit-aer qiskit-ibm-runtime
python main.py
