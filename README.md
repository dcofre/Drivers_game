# Drivers_game

This repository implements a numerical simulation and theoretical validation of symmetric equilibria in a queueing model with strategic drivers.

The project compares:

- Theoretical equilibrium predictions
- Simulation outcomes
- The infeasible region of the model

It generates graphical comparisons for different values of:
- p
- γ = v_c / v_l
- h
- n

---

# Project Structure

```
.
├── Cola.py
├── Conductores.py
├── main.py
└── README.md
```

---

# Model Overview

There are n drivers waiting in a queue.

At each period:

- A long trip arrives with probability p
- A short trip arrives with probability 1 − p

Each driver follows a threshold strategy (s_l, s_c):

- Accept a long trip if position ≥ s_l
- Accept a short trip if position ≥ s_c

The focus of the analysis is on symmetric equilibria in s_c.

The equilibrium condition requires that no unilateral deviation yields a strictly higher expected payoff.

---

# The Role of θ (Theta)

The parameter θ represents the probability governing the arrival order when two trips arrive simultaneously (one long and one short).

Specifically:

- With probability θ, the long trip arrives first.
- With probability 1 − θ, the short trip arrives first.

Thus:

- If θ = 1, the long trip always arrives before the short trip.
- If θ = 0, the short trip always arrives before the long trip.
- If 0 < θ < 1, the arrival order is stochastic.

In the current implementation, the model is evaluated under θ = 1, meaning the long trip always arrives first when both are available.

---

# File Description

## Conductores.py

Defines the class:

```python
class chofer:
```

Each driver object contains:

- Acceptance strategy (s_l, s_c)
- List of trip payoffs g
- List of inter-arrival times X
- Current queue position
- Re-entry time after serving a trip
- Acceptance/rejection logic

Main method:

```python
agregar_viaje()
```

This method determines whether the driver accepts a trip, updates payoffs, and schedules re-entry into the queue.

---

## Cola.py

Defines:

```python
class SimulacionCola:
```

This class simulates the full queue dynamics for r periods.

Key features:

- Stores performance metrics only for a selected driver i
- Handles re-entry scheduling
- Implements optimized queue updating
- Computes long-run payoff rate:

E = sum(g) / sum(X)

---

## main.py

Responsible for:

- Parameter grid exploration
- Analytical equilibrium computation
- Monte Carlo simulation
- Theory vs simulation comparison
- Figure generation

Main plotting function:

```python
graficar_comparacion_equilibrios_1h()
```

---

# Requirements

Python 3.9 or higher.

Required libraries:

```bash
pip install numpy matplotlib
```

---

# How to Run

From the project directory:

```bash
python main.py
```

The script will:

1. Iterate over values of p, γ, and h
2. Compute theoretical equilibria
3. Run simulations
4. Save figures in:

```
Resultados eq. para largo {n}/
```

---

# Main Parameters

In `main.py`, you can modify:

```python
n = 8          # Number of drivers
r = 100000     # Number of periods
cant_sim = 1   # Number of Monte Carlo runs
theta = 1      # Arrival order parameter
```

Grid resolution:

```python
c = 20         # discretization for p
gam = 20       # discretization for gamma
```

---

# Output

For each value of h, the script generates:

```
comparacion_sim_teo_h{h}n{n}.png
```

Each figure contains:

- Left panel: Simulation results
- Right panel: Theoretical predictions
- Shaded infeasible region
- Color-coded equilibrium values of s_c

---

# Reproducibility

The simulation relies on Python's random module.

For reproducible results, add at the beginning of `main.py`:

```python
import random
random.seed(123)
```

---

# Notes

- The implementation is optimized to store metrics only for driver i.
- The infeasible region is computed analytically.
- The current analysis is performed under θ = 1.
