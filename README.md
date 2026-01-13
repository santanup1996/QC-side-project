# Variational benchmarking of a 2‑qubit light–matter Hamiltonian (local + IBM Quantum)

This mini-project benchmarks a simple 2‑qubit light–matter Hamiltonian using:
- Local **noiseless** primitives (Aer `EstimatorV2`)
- A simple **local noise model** (Aer depolarizing + readout error)
- **IBM Quantum hardware** via Runtime `EstimatorV2`, comparing **raw** vs **mitigated** runs

## Model

$H(g)=\frac{\omega_c}{2}\,ZI + \frac{\omega_e}{2}\,IZ + g\,XX$

Conventions match Qiskit's Pauli strings (`Pauli("ZI")` acts on $q_1\otimes q_0$).

## Contents

- `notebooks/QC_side_project_clean.ipynb` — main notebook (clean, reproducible)
- `figures/` — figures saved by the notebook (`savefig(...)`)
- `ibm_runs/` — saved hardware job metadata + `PrimitiveResult` JSON (so the notebook can be re-run without consuming QPU time)

## Reproducing results

### 1) Local (noiseless + noisy) sections
Run the notebook from the top — the local sections require only:
- `qiskit`
- `qiskit-aer`
- `numpy`, `matplotlib`

### 2) IBM hardware section
**Recommended for public repos:** keep `already_computed=True` and include your saved `ibm_runs/` folder.

If you want to re-run hardware jobs:

1. Configure IBM Runtime access once (do **not** commit tokens):
   - Use `QiskitRuntimeService.save_account(...)` locally
2. In the notebook, set:
   - `INSTANCE_NAME = "<your instance>"`
   - `BACKEND_NAME = "<backend>"`

The notebook automatically saves job metadata and `PrimitiveResult` JSON under:
- `ibm_runs/light_matter_g_grid_raw/`
- `ibm_runs/light_matter_g_grid_mitigated/`

## Notes

- The runtime comparison uses `job.metrics()["usage"]["quantum_seconds"]` when available.
- Plot colors are consistent throughout:
  - Ansatz A: **blue**
  - Ansatz B: **orange**
  - Local Noise Model or, IBM hardware: **red**
  - Exact reference: **black**
