---
title: Correcting quantum errors
meta_title: ''
description: this is meta description
date: 2022-04-04T05:00:00Z
image: https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Quantum_error_correction_of_bit_flip_using_three_qubits.svg/1024px-Quantum_error_correction_of_bit_flip_using_three_qubits.svg.png
categories:
  - Quantum Error Correction
author: QEC Team
tags:
  - Error Correction
  - Qiskit
draft: false
---
# Quantum Error Correction Concepts in Qiskit

## Introduction

Quantum error correction (QEC) is essential in quantum computing to protect quantum information from errors due to decoherence and other quantum noise. Qiskit provides a framework to simulate and understand QEC codes.

---

## Classical Repetition Codes

A classical repetition code encodes a single bit into multiple bits to detect and correct errors.

### Encoding and Decoding

For a 3-bit repetition code:

- Encode 0 as `000` and 1 as `111`
- Decode by majority vote

### In Qiskit

You can simulate this using classical bits, or a simple quantum circuit with multiple qubits:

```python
from qiskit import QuantumCircuit
qc = QuantumCircuit(3, 1)
qc.cx(0, 1)
qc.cx(0, 2)
```

---

## Binary Symmetric Channels

A Binary Symmetric Channel (BSC) flips a bit with a certain probability \\\\\\\\\\\\\\\\( p \\\\\\\\\\\\\\\\).

### In Qiskit

You can simulate a BSC using the `NoiseModel`:

```python
from qiskit.providers.aer.noise import pauli_error
error = pauli_error([('X', p), ('I', 1 - p)])
```

---

## Repetition Code for Qubits

The quantum version of the repetition code uses entanglement and measurement.

### Encoding

To encode \\\\\\\\\\\\\\\\( |0\\\\\\\\\\\\\\\\rangle \\\\\\\\\\\\\\\\) as \\\\\\\\\\\\\\\\( |000\\\\\\\\\\\\\\\\rangle \\\\\\\\\\\\\\\\):

```python
qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 2)
```

### Bit-flip Error Detection

Measure syndromes to detect bit-flips:

```python
qc.cx(0, 1)
qc.cx(0, 2)
qc.measure_all()
```

---

## Phase-flip Errors

To detect phase-flips, use Hadamard gates to switch to the X-basis.

### Modified Repetition Code for Phase-Flip Errors

Add Hadamard gates before and after the bit-flip repetition circuit.

```python
qc.h([0, 1, 2])
# Apply bit-flip code
qc.cx(0, 1)
qc.cx(0, 2)
qc.h([0, 1, 2])
```

---

## The 9-Qubit Shor Code

Combines bit-flip and phase-flip protection using 9 qubits.

### Code Description

Encodes one logical qubit into 9 physical qubits by first using a phase-flip code then a bit-flip code.

### Errors and CNOT Gates

CNOT gates are used to entangle qubits and propagate errors.

### Correcting Bit-Flip Errors

Use parity checks on triplets to detect and correct.

### Correcting Phase-Flip Errors

Use Hadamards + parity checks on groups of 3 qubits.

### Simultaneous Bit- and Phase-Flip Errors

The Shor code corrects both by treating them sequentially.

---

## Random Errors

Any noise model may cause random errors.

### Discretization of Errors

Continuous errors can be mapped to X, Y, Z (Pauli) errors.

### Unitary Qubit Errors

A small rotation, e.g. \\\\\\\\\\\\\\\\( e^{-i \\\\\\\\\\\\\\\\theta X} \\\\\\\\\\\\\\\\), can be approximated as a probabilistic X error.

### Arbitrary Qubit Errors

Any error \\\\\\\\\\\\\\\\( E \\\\\\\\\\\\\\\\) can be decomposed as:
\\\\\\\\\\\\\\\\[ E = \\\\\\\\\\\\\\\\alpha I + \\\\\\\\\\\\\\\\beta X + \\\\\\\\\\\\\\\\gamma Y + \\\\\\\\\\\\\\\\delta Z \\\\\\\\\\\\\\\\]

---

## Generalizations

Advanced codes like the Steane and surface codes generalize these concepts. Qiskit supports simulation via the `qiskit.providers.aer.noise` and `qiskit.qec` modules.

---

## Visual Example

[![3-Qubit Repetition Code](https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Quantum_error_correction_of_bit_flip_using_three_qubits.svg/1024px-Quantum_error_correction_of_bit_flip_using_three_qubits.svg.png "Title test 1")](https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Quantum_error_correction_of_bit_flip_using_three_qubits.svg/1024px-Quantum_error_correction_of_bit_flip_using_three_qubits.svg.png)

---

To explore further, check out Qiskit's [QEC tutorial](https://qiskit.org/textbook/ch-error-correction/repetition-code.html).
