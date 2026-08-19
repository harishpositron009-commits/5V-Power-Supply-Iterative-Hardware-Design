# 05 — Simulation & Validation

## Objective

After developing and analyzing the first working circuit, the next step was to verify whether the expected theoretical behaviour could also be observed in simulation.

**LTspice** was selected for circuit simulation.

The purpose of simulation was not simply to check whether the circuit produced 5 V.

The objectives were to:

- Observe the behaviour of individual stages
- Compare theoretical calculations with simulated results
- Examine rectification and filtering
- Observe output ripple
- Study circuit behaviour under different loads
- Verify the regulator stage
- Identify unexpected behaviour before PCB implementation

The simulation therefore acts as another layer of evidence between:

**Theory → Simulation → Physical Implementation**

---

# Simulation Architecture

The circuit was modelled stage by stage:

```text
AC Source
    ↓
Transformer / Low-Voltage AC Source
    ↓
Bridge Rectifier
    ↓
Filter Capacitor
    ↓
7805 Regulator
    ↓
Load
