# Interactive Computing Architecture Simulator

## 1. Project Overview

Build an educational web application that visually compares how different computing architectures process data:

1. CPU / Classical Computing
2. GPU / NVIDIA CUDA
3. HPC-style Parallel Computing
4. QPU / Quantum Computing
5. Quantum Noise + Error Correction
6. Basic AI-assisted Quantum Error Mitigation

The project is primarily a **Python project**.

Python/FastAPI performs the actual computation and simulation.

The frontend uses only:

* HTML
* CSS
* Vanilla JavaScript
* SVG / Canvas where required

Do NOT use React, Vue, Angular, Tailwind, Bootstrap, Electron, database systems, authentication, Docker, microservices, or other unnecessary infrastructure.

The goal is **learning and visualization**, not building a production platform.

---

# 2. Main Goal

The application should answer:

> How does information get represented, processed, parallelized, affected by errors, and converted into an output in different computing architectures?

The application must make the internal processing visible.

Do not simply display final numerical results.

While processing, the frontend must visually explain:

* what is happening
* why it is happening
* how it is happening
* what changed from the previous step
* what the result means

Example:

```text
Input
  ↓
Data divided
  ↓
Worker assigned
  ↓
Operation executed
  ↓
Results combined
  ↓
Output
```

The application should animate this flow.

---

# 3. Core Design Principle

Every module should follow the same basic structure:

```text
INPUT
  ↓
PROCESSING
  ↓
INTERNAL STATE
  ↓
OUTPUT
  ↓
VISUAL EXPLANATION
```

For quantum computing:

```text
INITIAL STATE
  ↓
QUANTUM GATE
  ↓
STATEVECTOR CHANGES
  ↓
AMPLITUDES / PHASES
  ↓
MEASUREMENT
  ↓
PROBABILITY DISTRIBUTION
```

For GPU:

```text
INPUT DATA
  ↓
COPY TO GPU
  ↓
THREADS CREATED
  ↓
THREADS PROCESS DATA
  ↓
RESULT COPIED BACK
  ↓
OUTPUT
```

For HPC:

```text
LARGE JOB
  ↓
DIVIDE INTO CHUNKS
  ↓
ASSIGN TO WORKERS
  ↓
PARALLEL PROCESSING
  ↓
COLLECT RESULTS
  ↓
MERGE
```

---

# 4. Frontend Layout

The application should have a simple laboratory-style interface.

## Main Layout

```text
+------------------------------------------------------+
| COMPUTING LAB                                        |
| Interactive Computing Architecture Simulator        |
+------------------------------------------------------+

[ CPU ] [ GPU / CUDA ] [ HPC ] [ QUANTUM ]

--------------------------------------------------------

                 VISUALIZATION AREA

--------------------------------------------------------

Explanation / Processing Flow

Step 1:
Input data received.

Step 2:
Data divided into 4 chunks.

Step 3:
Each CUDA thread processes one element.

Step 4:
Results are copied back to CPU.

--------------------------------------------------------

Controls

[ Run ] [ Step ] [ Reset ]

Input Size: [ 1000 ]
Workers:   [ 4 ]

--------------------------------------------------------

Metrics
Execution Time
Operations
Workers / Threads
Memory
```

Keep the interface clean.

No large dashboard with unnecessary cards.

---

# 5. Frontend: Processing Explanation Panel

This is one of the most important parts of the project.

Whenever something happens, show a human-readable explanation.

Example:

```text
PROCESSING FLOW

1. Input received
   "The array contains 8 elements."

2. Work assigned
   "Each CUDA thread receives one element."

3. Parallel execution
   "Multiple threads execute the operation simultaneously."

4. Result returned
   "The GPU returns the processed array to the CPU."
```

For quantum:

```text
1. Initial state
   "The qubit starts in |0⟩."

2. Hadamard gate
   "The H gate changes the amplitudes of |0⟩ and |1⟩."

3. New state
   "|ψ⟩ = 0.707|0⟩ + 0.707|1⟩"

4. Measurement
   "The state is measured probabilistically."
```

The explanation text should update dynamically according to the current operation.

Do not hard-code a huge paragraph for every screen.

The backend should return structured information such as:

```json
{
  "step": 2,
  "title": "Hadamard Gate Applied",
  "explanation": "The H gate changes the amplitudes of the two basis states.",
  "state": "...",
  "visual_type": "statevector"
}
```

The frontend renders this.

---

# 6. CPU Module

## Purpose

Demonstrate basic classical computation.

Use simple array operations rather than complex algorithms.

Example:

```python
C[i] = A[i] + B[i]
```

Frontend visualization:

```text
A: [1] [2] [3] [4]

B: [5] [6] [7] [8]

      ↓

CPU processing

[1]+[5]
[2]+[6]
[3]+[7]
[4]+[8]

      ↓

C: [6] [8] [10] [12]
```

Show which element is currently being processed.

Controls:

* data size
* run
* step
* reset

Metrics:

* number of operations
* execution time

Purpose of this module:

> Establish a classical baseline before introducing parallel and quantum computation.

---

# 7. GPU / NVIDIA CUDA Module

This module must help the student understand actual CUDA concepts.

Important CUDA concepts to demonstrate:

* Host
* Device
* Kernel
* Thread
* Block
* Grid
* Device memory
* Host ↔ Device transfer
* Parallel execution

Do NOT hide everything behind a high-level GPU library.

Use **Numba CUDA** so the CUDA kernel remains visible and understandable in Python.

Example conceptual kernel:

```python
@cuda.jit
def vector_add(a, b, c):
    i = cuda.grid(1)

    if i < c.size:
        c[i] = a[i] + b[i]
```

The code should remain simple enough to explain line-by-line.

## GPU Visual

```text
CPU / HOST

Input Array
   |
   | copy
   v

GPU / DEVICE

Grid
+-----------------------------+
| Block 0 | Block 1 | Block 2 |
+-----------------------------+

Threads:

T0 -> element 0
T1 -> element 1
T2 -> element 2
T3 -> element 3
...

   |
   | copy result
   v

CPU / HOST
Output
```

Animate data moving from CPU memory to GPU memory.

Then animate threads processing elements.

The visualizer should clearly show:

```text
HOST
 ↓
MEMORY COPY
 ↓
DEVICE
 ↓
KERNEL
 ↓
THREADS
 ↓
RESULT
 ↓
HOST
```

The explanation panel should say what each stage means.

Example:

```text
Kernel launched.

A kernel is a function executed by many GPU threads.

Thread 12 is processing element 12.
```

---

# 8. GPU Fallback

The project must run even if NVIDIA CUDA is unavailable.

Implement:

```text
CUDA available
    ↓
Use CUDA implementation

CUDA unavailable
    ↓
Use CPU simulation
```

The UI should display:

```text
CUDA STATUS: AVAILABLE
```

or:

```text
CUDA STATUS: NOT AVAILABLE
Using CPU simulation mode.
```

Never fake GPU measurements.

If actual CUDA is unavailable, clearly label the visualization as a simulation/fallback.

---

# 9. HPC Module

Do not pretend that multiprocessing on one laptop is a real supercomputer.

This module is an **HPC/distributed-computation concept simulator**.

Use Python's:

```python
multiprocessing
```

or `concurrent.futures.ProcessPoolExecutor`.

Demonstrate:

* job
* chunk
* worker
* parallel processing
* result collection
* merge

Example:

```text
Large Array

[1 2 3 4 5 6 7 8]

        ↓

Split

Worker 1 → [1 2]
Worker 2 → [3 4]
Worker 3 → [5 6]
Worker 4 → [7 8]

        ↓

Process

        ↓

Collect

        ↓

Final Result
```

Allow the user to change:

```text
Number of workers:
1
2
4
8
```

Display measured execution time.

The UI must explicitly state:

> This is a local parallel-processing simulation of an HPC-style workload, not a real multi-node supercomputer.

---

# 10. Quantum Module

This is the most educational module.

Do NOT immediately depend on Qiskit for all quantum logic.

Implement a small quantum simulator using:

```text
Python
NumPy
```

Qiskit/Aer may be used separately to verify results.

The custom simulator should demonstrate the mathematics directly.

---

# 11. Quantum Concepts to Implement

Implement only:

1. Qubit
2. Statevector
3. X gate
4. H gate
5. Z gate
6. CNOT
7. Measurement
8. Superposition
9. Interference
10. Entanglement
11. Simple noise
12. Simple error correction

Do not implement dozens of gates unless necessary.

---

# 12. Quantum State Representation

Represent an n-qubit state using a NumPy complex vector.

Examples:

1 qubit:

```text
|ψ⟩ = α|0⟩ + β|1⟩
```

Statevector:

```text
[α, β]
```

2 qubits:

```text
|ψ⟩ =
a|00⟩ +
b|01⟩ +
c|10⟩ +
d|11⟩
```

Statevector:

```text
[a, b, c, d]
```

The frontend should display:

```text
BASIS STATE      AMPLITUDE       PROBABILITY

|00⟩             +0.707          50%
|01⟩              0.000           0%
|10⟩              0.000           0%
|11⟩             +0.707          50%
```

Probability is:

```text
|amplitude|²
```

---

# 13. Quantum Step-by-Step Visualizer

The user must be able to click:

```text
[ Step ]
```

and advance one operation at a time.

Example:

```text
Initial

q0 = |0⟩
```

Click Step:

```text
H applied

q0 = 0.707|0⟩ + 0.707|1⟩
```

Click Step again:

```text
Measurement probabilities

P(0) = 50%
P(1) = 50%
```

Do not jump directly to the final answer.

---

# 14. Superposition Visualization

Show:

```text
|0⟩
 |
 | H
 ↓

|ψ⟩

|0⟩  ██████████  0.707
|1⟩  ██████████  0.707
```

Explanation:

```text
The qubit state is represented using amplitudes
for the basis states |0⟩ and |1⟩.

The amplitudes determine the probabilities
observed during measurement.
```

Avoid misleading text such as:

> "The qubit is literally both 0 and 1."

Use amplitude/state terminology.

---

# 15. Interference Visualization

Use a simple circuit that demonstrates constructive and destructive interference.

The UI should show amplitude bars before and after the operation.

Example:

```text
Before

|0⟩   +0.707
|1⟩   +0.707

        ↓

Quantum gates

        ↓

After

|0⟩   +1.000
|1⟩    0.000
```

The explanation panel:

```text
Amplitudes combine.

Some paths reinforce one another.
Some paths cancel one another.

This is constructive and destructive interference.
```

Use animation when amplitudes change.

---

# 16. Entanglement Visualization

Use the Bell-state circuit:

```text
q0 ── H ──●────
          │
q1 ───────X────
```

Show:

```text
|00⟩ = 50%
|11⟩ = 50%
|01⟩ = 0%
|10⟩ = 0%
```

Then provide:

```text
[ Run 100 Measurements ]
```

Example histogram:

```text
00  ███████████████████
01
10
11  ██████████████████
```

The explanation should state:

```text
The two qubits are represented by a joint quantum state.

Measurement outcomes are correlated.
```

Do not use vague explanations such as:

> "The particles communicate instantly."

---

# 17. Quantum Noise

Implement simple stochastic noise.

At minimum:

```text
Bit flip
Phase flip
```

Allow the user to select:

```text
Noise probability:
0%
1%
5%
10%
25%
```

Example:

```text
Ideal

|0⟩ ───────────────► |0⟩


Noise

|0⟩ ───── X ───────► |1⟩
         ↑
       error
```

The simulator should run multiple measurements so the effect of noise becomes visible statistically.

---

# 18. Error Correction

Implement a simple 3-qubit repetition-code demonstration.

Logical state:

```text
000
```

Inject one bit-flip error:

```text
001
```

Then majority logic:

```text
0 0 1
  ↓
majority = 0
```

Restore:

```text
000
```

The UI must clearly label this:

> Simple bit-flip repetition-code demonstration.

Do not describe it as a complete fault-tolerant quantum computer.

---

# 19. AI-Assisted Error Mitigation

This is a small experimental extension.

Do not build a large AI system.

Use a simple machine-learning model from:

```python
scikit-learn
```

Generate synthetic noisy quantum results.

Features can include:

```text
noise probability
circuit depth
measured probability
number of shots
```

The model predicts an estimate of the ideal result.

Visualize:

```text
IDEAL
1.00

NOISY
0.82

AI ESTIMATE
0.94
```

The frontend explanation:

```text
The ML model does not physically repair a qubit.

It learns a relationship between noisy observations
and the expected ideal result.

This is an error-mitigation experiment.
```

Keep this distinction explicit.

---

# 20. Network / REST API Learning

The project must use an actual REST API so the student can understand networking.

Architecture:

```text
Browser
   |
   | HTTP request
   v
FastAPI
   |
   v
Python computation
   |
   v
JSON response
   |
   v
Browser
```

The frontend should communicate using:

```javascript
fetch()
```

Example:

```javascript
fetch("/api/quantum/step", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify(requestData)
});
```

The student should be able to explain:

```text
HTTP request
→ route
→ JSON body
→ FastAPI
→ Python computation
→ JSON response
→ JavaScript
→ visualization
```

Do not introduce WebSockets unless they are actually necessary.

For this project, REST is sufficient.

---

# 21. API Design

Use simple REST endpoints.

## System

```text
GET /api/system/info
```

Returns:

```json
{
  "python_version": "...",
  "cuda_available": true,
  "cpu_count": 8
}
```

## CPU

```text
POST /api/cpu/run
```

## GPU

```text
POST /api/gpu/run
```

## HPC

```text
POST /api/hpc/run
```

## Quantum

```text
POST /api/quantum/create
POST /api/quantum/step
POST /api/quantum/measure
POST /api/quantum/reset
```

## Noise

```text
POST /api/quantum/noise
```

## Error Correction

```text
POST /api/quantum/error-correction
```

## AI

```text
POST /api/quantum/mitigation
```

Keep request and response structures simple.

---

# 22. Backend Structure

Use:

```text
backend/
│
├── main.py
│
├── api/
│   ├── system.py
│   ├── cpu.py
│   ├── gpu.py
│   ├── hpc.py
│   └── quantum.py
│
├── engines/
│   ├── cpu_engine.py
│   ├── gpu_engine.py
│   ├── hpc_engine.py
│   │
│   └── quantum/
│       ├── state.py
│       ├── gates.py
│       ├── circuit.py
│       ├── measurement.py
│       ├── noise.py
│       └── error_correction.py
│
└── ml/
    └── error_mitigation.py
```

Do not create dozens of files.

Each file should have a clear purpose.

---

# 23. Frontend Structure

```text
frontend/
│
├── index.html
│
├── css/
│   └── style.css
│
└── js/
    ├── app.js
    ├── api.js
    ├── cpu.js
    ├── gpu.js
    ├── hpc.js
    ├── quantum.js
    ├── visualizer.js
    └── explanation.js
```

---

# 24. Visualization Rules

The frontend must prioritize understanding over decoration.

Use:

### SVG

For:

* circuit diagrams
* wires
* gates
* nodes
* architecture diagrams

### Canvas

For:

* amplitude bars
* probability charts
* animated particles/data
* performance graphs

Normal HTML/CSS:

For:

* buttons
* controls
* explanations
* metrics
* tables

No 3D graphics.

No Three.js.

No WebGL unless absolutely necessary.

---

# 25. Visual Language

Use consistent visuals across every module.

### Data

Represent data as blocks:

```text
[ 1 ] [ 2 ] [ 3 ] [ 4 ]
```

### Worker

```text
┌─────────┐
│ Worker 1│
└─────────┘
```

### GPU thread

```text
T0
T1
T2
T3
```

### Quantum state

```text
|00⟩
|01⟩
|10⟩
|11⟩
```

### Processing

Animate data moving between components.

Example:

```text
CPU
 ↓
GPU
 ↓
THREADS
 ↓
GPU
 ↓
CPU
```

---

# 26. Global Step / Run / Reset Controls

Every experiment should have:

```text
[ STEP ]
[ RUN ]
[ PAUSE ]
[ RESET ]
```

### STEP

Execute exactly one conceptual operation.

### RUN

Execute the complete experiment.

### RESET

Return to the initial state.

This is especially important for the quantum module.

---

# 27. Explanation Engine

Create a small frontend/backend explanation mechanism.

Every processing step should return:

```json
{
  "step": 3,
  "title": "Threads Executing",
  "description": "Each thread processes one element.",
  "technical_detail": "Thread index is calculated using cuda.grid(1)."
}
```

Frontend:

```text
Step 3 — Threads Executing

Each thread processes one element.

Technical:
cuda.grid(1) calculates the global thread index.
```

This allows the application to teach both:

### Simple explanation

“What is happening?”

and

### Technical explanation

“How is the code doing it?”

---

# 28. Code Explainability Requirement

Every important code section must be understandable during viva.

Avoid:

* complex abstractions
* design-pattern-heavy architecture
* unnecessary decorators
* metaprogramming
* unnecessary classes
* over-engineered dependency injection
* complicated async systems
* unnecessary third-party libraries

Prefer:

```python
def apply_gate(state, gate):
    return gate @ state
```

over a complicated quantum framework.

Prefer:

```python
executor.map(process_chunk, chunks)
```

over a custom distributed job scheduler.

Prefer:

```python
fetch(...)
```

over a complex frontend state-management library.

---

# 29. Required Learning Areas

The implementation should help the student understand these topics.

## Python

* NumPy arrays
* complex numbers
* matrix multiplication
* multiprocessing
* REST APIs
* JSON
* FastAPI
* basic ML with scikit-learn

## CUDA

* Host
* Device
* Memory transfer
* Kernel
* Thread
* Block
* Grid
* Thread indexing
* Parallel execution

## Networking

* HTTP
* GET
* POST
* JSON
* client/server model
* REST API
* request/response

## Quantum Computing

* bit vs qubit
* basis states
* statevector
* amplitudes
* probability
* phase
* quantum gates
* superposition
* interference
* entanglement
* measurement
* quantum noise
* basic error correction
* error mitigation

---

# 30. Project Scope Restrictions

Do NOT add:

* authentication
* database
* user accounts
* cloud deployment
* Kubernetes
* Docker
* Redis
* PostgreSQL
* React
* Tailwind
* complex AI agents
* LLM integration
* advanced QML algorithms
* real quantum hardware requirement
* real HPC cluster requirement

These do not contribute enough to the learning objective.

---

# 31. Optional Qiskit Usage

Qiskit can be installed as a **verification/reference tool**.

Example:

```text
Custom NumPy Quantum Simulator
            |
            | compare
            v
         Qiskit
```

Run the same small circuit through both implementations.

Display:

```text
Custom Simulator Result: ...
Qiskit Result: ...
Match: YES
```

Do not make Qiskit responsible for everything.

The student should understand the underlying matrix/state operations.

---

# 32. Example Complete User Flow

## Example: Quantum experiment

User selects:

```text
QUANTUM
```

Then:

```text
Qubits: 2

Circuit:

q0 ── H ──●────
          │
q1 ───────X────

[STEP] [RUN] [RESET]
```

Step 1:

```text
Initial state

|00⟩ = 1
```

Step 2:

```text
H applied to q0

State:

|00⟩ = 0.707
|01⟩ = 0
|10⟩ = 0.707
|11⟩ = 0
```

Explanation:

```text
The Hadamard gate creates a superposition
between the basis states of q0.
```

Step 3:

```text
CNOT applied

State:

|00⟩ = 0.707
|11⟩ = 0.707
```

Explanation:

```text
The CNOT correlates the two qubits,
creating an entangled Bell state.
```

Step 4:

```text
Measurement

00 → 51%
11 → 49%
```

Explanation:

```text
Quantum measurement produces a probabilistic outcome.
Repeated measurements reveal the probability distribution.
```

This is the level of interaction expected throughout the application.

---

# 33. Performance Experiments

Include a small experimental section.

Allow:

```text
Input size
Number of workers
Number of measurements
Noise probability
```

Record:

```text
execution time
operations
workers
threads
measurement statistics
```

Generate graphs.

Example:

```text
Execution Time vs Input Size

CPU
GPU
HPC-style multiprocessing
```

For quantum:

```text
Noise Probability vs Error Rate
```

Do not claim that one architecture is universally faster.

The goal is to demonstrate **different computational models and trade-offs**.

---

# 34. Important Scientific Accuracy Rules

The application must not use misleading statements such as:

```text
"Quantum computers try every answer simultaneously."
```

Instead use:

```text
"Quantum algorithms manipulate amplitudes of multiple basis states,
and interference changes the probability of measurement outcomes."
```

Do not say:

```text
"GPU = HPC"
```

Instead:

```text
"A GPU is a parallel accelerator.
HPC refers to high-performance computing systems and workloads,
which may use CPUs, GPUs, or both."
```

Do not say:

```text
"AI fixes quantum computers."
```

Instead:

```text
"Machine learning can be used to estimate or mitigate the effect
of noise in measurement results."
```

---

# 35. Final Project Navigation

The application should contain:

```text
HOME

├── CPU LAB
│
├── GPU / CUDA LAB
│
├── HPC LAB
│
├── QUANTUM LAB
│   ├── Qubit
│   ├── Gates
│   ├── Superposition
│   ├── Interference
│   ├── Entanglement
│   ├── Measurement
│   └── Noise
│
├── ERROR CORRECTION
│
├── AI ERROR MITIGATION
│
└── COMPARISON
```

The Comparison page should summarize the concepts learned rather than simply declaring a winner.

---

# 36. Recommended Build Order

Do not build everything at once.

Build in this order:

```text
1. FastAPI skeleton
2. HTML/CSS/JS frontend
3. REST communication
4. CPU simulation
5. GPU CUDA simulation
6. HPC multiprocessing simulation
7. Basic quantum statevector
8. Quantum gates
9. Measurement
10. Interference
11. Entanglement
12. Noise
13. Error correction
14. AI error mitigation
15. Comparison page
16. Final polishing
```

After each stage, verify that the student can explain the code before adding the next stage.

---

# 37. Definition of Done

The project is complete when:

* The browser communicates with FastAPI.
* CPU computation is visible step-by-step.
* CUDA execution is demonstrated when NVIDIA CUDA is available.
* CPU fallback works when CUDA is unavailable.
* HPC-style multiprocessing is visualized.
* A custom NumPy quantum statevector simulator works.
* H, X, Z and CNOT gates work.
* Superposition is visualized.
* Interference is visualized.
* Entanglement is visualized.
* Measurement is probabilistic.
* Noise can be injected.
* A simple repetition-code correction is demonstrated.
* A basic ML error-mitigation experiment works.
* Every major operation has a visual explanation.
* The student can explain every major backend and frontend component.
* No unnecessary frameworks or infrastructure are included.

---

# 38. Core Principle for the Coding Assistant

When generating this project:

> **Choose the simplest implementation that clearly demonstrates the concept.**

Do not add features merely because they are technically possible.

Every dependency, file, class, endpoint, and abstraction must have a clear educational purpose.

The project should be something an MSc Computer Science student can understand completely and explain during a viva.
