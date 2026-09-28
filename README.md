# Interactive-Comparative-Simulation-of-Classical-GPU-HPC-and-Quantum-Computing
How do different computing paradigms represent, process, parallelize, and recover data?


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
                       

