# Interactive-Comparative-Simulation-of-Classical-GPU-HPC-and-Quantum-Computing
### An interactive simulator that visually demonstrates how classical and quantum computing process information differently


Tentative Architecture Frontend:
~~~
                    Browser
        ┌──────────────────────────┐
        │ HTML                     │
        │ CSS                      │
        │ JavaScript               │
        │                          │
        │ Canvas / SVG             │
        │ animations               │
        └────────────┬─────────────┘
                     │ REST
                     ▼
        ┌──────────────────────────┐
        │       FastAPI            │
        │                          │
        │ /cpu                     │
        │ /gpu                     │
        │ /hpc                     │
        │ /quantum                 │
        │ /noise                   │
        │ /error-correction        │
        │ /ai-mitigation           │
        └────────────┬─────────────┘
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
    NumPy         Numba        Quantum Engine
                   CUDA          ↓
                               NumPy
                               + optional Qiskit

~~~
                       

