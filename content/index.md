---
title: Smoothed Particle Hydrodynamics (SPH)
---

Welcome to the **Smoothed Particle Hydrodynamics (SPH)** digital garden — an interconnected repository of computational algorithms, theoretical derivations, and high-performance simulation notes for astrophysical fluid dynamics.

---

## Overview & Mathematical Foundations

Smoothed Particle Hydrodynamics is a mesh-free Lagrangian method where continuous fluid fields $A(\mathbf{r})$ are approximated via a set of discrete particles with smoothing kernel $W(\mathbf{r} - \mathbf{r}', h)$:

$$A(\mathbf{r}) \approx \int A(\mathbf{r}') W(\mathbf{r} - \mathbf{r}', h) \, d^3\mathbf{r}' \approx \sum_{b} m_b \frac{A_b}{\rho_b} W(\mathbf{r} - \mathbf{r}_b, h)$$

### Core Principles
* **Lagrangian Conservation**: Mass, momentum, and angular momentum are inherently conserved without grid advection errors.
* **Kernel Properties**: Symmetric, compact support ($W = 0$ for $r > 2h$), and strict normalization $\int W \, dV = 1$ (partition of unity).
* **Gradient Operators**: Symmetric discretisation ensures exact anti-symmetry of inter-particle forces:
  $$\nabla A_a \approx \sum_b m_b \left( \frac{A_a}{\rho_a^2} + \frac{A_b}{\rho_b^2} \right) \nabla_a W_{ab}$$

---

## Computational Engine Architecture

1. **Neighbor Search & Grid Hashing**:
   * Spatial linked-cell spatial decomposition for $O(N)$ neighbor retrieval.
   * Compact stencil evaluation within smoothing radius $2h$.

2. **Hydrodynamic Stepping**:
   * **Density Summation**: Monaghan quintic spline / cubic B-spline kernels.
   * **Equation of State (EOS)**: Isothermal and ideal gas equations of state ($P = (\gamma - 1)\rho u$).
   * **Monaghan-Gingold Artificial Viscosity**: Shock capturing via standard $\alpha, \beta$ bulk viscosity terms with Balsara switches.

3. **Time Integration**:
   * Symplectic leapfrog (Kick-Drift-Kick / Drift-Kick-Drift) schemes with Courant-Friedrichs-Lewy (CFL) adaptive timestepping:
     $$\Delta t \le C_{\text{CFL}} \min_a \left( \frac{h_a}{c_{s,a} + v_{\text{sig},a}} \right)$$

---

## Astrophysical Applications

* **Protoplanetary Disks**: Evolution of gas disks perturbed by embedded protoplanets, spiral arm excitation, and gap opening.
* **Polytropic Stellar Equilibria**: Lane-Emden relaxation and non-axisymmetric core collapse.
* **Shock Tubes & Blast Waves**: 1D/2D Sod shock verification and Sedov-Taylor blast comparisons.

---

## Literature & Reference Library

* **Zotero Group**: [d57-sph Zotero Library](https://www.zotero.org/groups/6778487/d57-sph) &mdash; core bibliography covering hydrodynamic foundations, artificial viscosity, and GPU acceleration.
