---
name: project_context
description: Core research agenda, tools, and status for the Low-Rank Recovery and Inference project
type: project
---

**Project:** Low-Rank Recovery and Inference for Sparsely Observed Treatment Effects

**Core problem:** Matrix recovery and inference under sparsely observed columns of estimated treatment effects. Goal: recover the full matrix and conduct valid statistical inference (mean and SE) under a low-rank structural assumption.

**Tools:** LaTeX/Beamer (talks), Python (NumPy/SciPy, statsmodels, scikit-learn, matplotlib/seaborn)

**Target outlet:** Top-5 economics journal

**Status as of 2026-03-30:** Clean start. No slides or code written yet.

**Planned talk deck structure:**
1. Problem Setup & Motivation — low-rank model, sparse observations, TE estimation
2. Methodology — recovery algorithm, inference procedure
3. Theory — main theorems, asymptotic results
4. Simulations & Application — finite-sample evidence, empirical illustration

**Why:** Matrix-completion-style recovery motivated by treatment effect estimation where only a sparse subset of unit-treatment columns is observed. The low-rank assumption reflects latent factor structure common in panel/network settings.

**How to apply:**
- All theoretical claims must be traceable to specific assumptions and results
- Simulation DGPs must match the theoretical model exactly
- Every estimator on slides must have a corresponding Python implementation
