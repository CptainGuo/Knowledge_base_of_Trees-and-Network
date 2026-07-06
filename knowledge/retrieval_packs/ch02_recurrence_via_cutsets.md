# Retrieval Pack: Proving Recurrence via Cutsets

Source: Lyons and Peres, *Probability on Trees and Networks*, Chapter 2.

## Intended Queries

- "How do I prove a network is recurrent using electrical networks?"
- "Use Nash-Williams inequality to show recurrence."
- "What trick converts recurrence into a cutset estimate?"
- "How does effective resistance relate to transience?"

## Core Dictionary

Random walk on an infinite connected network is transient iff the effective
conductance from a vertex to infinity is positive. Equivalently, recurrence
means the effective resistance to infinity is infinite.

For a finite boundary `Z`,

```text
C(a <-> Z) = pi(a) * P_a[tau_Z < tau_a^+]
R(a <-> Z) = 1 / C(a <-> Z)
```

Passing to wired exhaustions gives `C(a <-> infinity)`.

## Main Trick

To prove recurrence, do not try to compute return probabilities directly.
Instead:

1. Choose an exhaustion around the root.
2. Find pairwise disjoint cutsets `Pi_k` separating the root from infinity.
3. Estimate the total conductance across each cutset:

```text
C(Pi_k) = sum_{e in Pi_k} c(e)
```

4. Apply Nash-Williams:

```text
R(a <-> infinity) >= sum_k 1 / C(Pi_k)
```

5. If the sum diverges, then `R(a <-> infinity) = infinity`, hence
`C(a <-> infinity) = 0`, hence the random walk is recurrent.

## Why It Works

A unit flow from the root to the boundary must cross every separating cutset.
Across each cutset, Cauchy-Schwarz forces an energy cost of at least
`1 / sum_{e in Pi} c(e)`. If cutsets are disjoint, these energy costs add.
Since current minimizes energy among all unit flows, this lower bounds the
effective resistance.

## Key Premises

- `def.lp.effective_conductance`: Effective conductance equals local
  conductance times escape probability.
- `def.lp.energy_of_flow`: Flow energy is `sum r(e) theta(e)^2`, and unit
  current energy equals effective resistance.
- `thm.lp.transience_effective_conductance`: Transience iff
  `C(a <-> infinity) > 0`.
- `thm.lp.thomson_principle`: Current uniquely minimizes energy among flows
  with the same sources and sinks.
- `thm.lp.nash_williams_inequality`: Disjoint cutsets give additive lower
  bounds on resistance.

## Agent Use Pattern

When the goal is "prove recurrence":

```text
Goal: show recurrent(G)
Retrieve:
  - transience_effective_conductance
  - nash_williams_inequality
  - cutset_resistance_lower_bound
Plan:
  - construct disjoint cutsets around the root
  - show sum_k 1 / total_conductance(Pi_k) diverges
  - conclude R_eff(root, infinity) = infinity
  - conclude C_eff(root, infinity) = 0
  - conclude recurrence
```

When the goal is "prove transience", this pack is usually not enough. Prefer a
dual pack based on constructing a finite-energy unit flow to infinity.

## Common Pitfalls

- The cutsets must be pairwise disjoint.
- Each cutset must separate the root from the target boundary or infinity.
- Nash-Williams is a lower bound on resistance, so it is naturally a recurrence
  tool.
- One explicit flow gives an upper bound on resistance, not a lower bound.
