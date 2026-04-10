# Plan: Adapt Workflow Configuration for Low-Rank Recovery Project

**Status:** DRAFT
**Date:** 2026-03-30
**Goal:** Adapt all template configuration files to fit the Low-Rank Recovery and Inference project

---

## Context

The user has forked the claude-code-my-workflow template. It ships with generic placeholders in CLAUDE.md, a domain-reviewer agent with template lenses, and R-centric code conventions. This plan adapts those files for a research project in matrix recovery and treatment effect inference, using Python (NumPy/SciPy, statsmodels, scikit-learn, matplotlib) and LaTeX/Beamer. The project targets top-5 econ journals. This is a clean start (no slides or code yet).

---

## Files to Modify

| File | Change |
|------|--------|
| `CLAUDE.md` | Fill placeholders, adapt for Python research project |
| `.claude/agents/domain-reviewer.md` | Customize 5 lenses for this research domain |
| `.claude/rules/r-code-conventions.md` | Replace with Python code conventions |

## Files to Create

| File | Purpose |
|------|---------|
| `.claude/state/personal-memory.md` | Gitignored local memory (project-specific setup) |
| `memory/user_profile.md` | User memory: research goals, collaboration preferences |
| `memory/project_context.md` | Project memory: research agenda, key decisions |
| `MEMORY.md` | Add pointers to new memory files |

---

## Step-by-Step Changes

### 1. Update `CLAUDE.md`

**Placeholders to fill:**
- `[YOUR PROJECT NAME]` → `Low-Rank Recovery and Inference for Sparsely Observed Treatment Effects`
- `[YOUR INSTITUTION]` → leave blank (user's choice)
- Branch: `main`

**Folder structure:** Add `Scripts/` for Python, `Figures/` already present. Remove references implying R-only.

**Commands section:** Replace R/Quarto commands that don't apply with Python simulation commands:
```bash
# Run simulation
python scripts/simulations.py

# Run tests
python -m pytest scripts/tests/
```
Keep LaTeX commands (still using Beamer). Keep Quarto section since /deploy skill still useful.

**Beamer environments:** Leave as placeholder — user hasn't defined any yet.

**Project state table:** Initialize with placeholder for first talk/slide deck:
```
| Deck | Beamer | Status | Key Content |
|------|--------|--------|-------------|
| 1: Problem Setup & Motivation | -- | Not started | Low-rank model, sparse obs, TE estimation |
| 2: Methodology | -- | Not started | Recovery algorithm, inference procedure |
| 3: Theory | -- | Not started | Main theorems, asymptotic results |
| 4: Simulations & Application | -- | Not started | Finite-sample evidence |
```

**Quality thresholds:** Keep 80/90/95 — appropriate for top-5 journal standard.

---

### 2. Customize `.claude/agents/domain-reviewer.md`

Replace generic template lenses with 5 research-specific lenses:

**Lens 1 — Matrix Algebra & Linear Algebra Rigor**
- Rank conditions stated explicitly?
- SVD properties invoked correctly (truncation error, spectral gap)?
- Operator/Frobenius norm bounds tight and labeled?
- Incoherence/RIP conditions stated when invoked?

**Lens 2 — Statistical Inference Correctness**
- Asymptotic normality conditions verified (CLT applicability)?
- Bias-variance decomposition correct?
- SE construction: conservative vs. oracle, plug-in vs. sandwich?
- Coverage guarantees: uniform vs. pointwise?
- Multiple testing / simultaneous inference addressed if needed?

**Lens 3 — Causal Inference Validity**
- Identification assumptions stated (SUTVA, overlap, unconfoundedness or instrument validity)?
- Distinction between ATE/ATT/CATE clear?
- Sensitivity to assumption violations discussed?
- Estimand defined before estimator?

**Lens 4 — Numerical Methods & Simulation Design**
- Algorithm convergence criteria stated?
- Initialization sensitivity discussed?
- Simulation DGP matches theoretical model assumptions?
- Monte Carlo repetitions, seed, and parameter grid documented?
- Finite-sample behavior consistent with asymptotic theory?

**Lens 5 — Economic Interpretation & Policy Relevance**
- Treatment effect estimates interpretable in economic units?
- Magnitudes benchmarked against prior literature?
- Limitations and external validity discussed?
- Does the slide motivate WHY this matters for policy/practice?

---

### 3. Replace `.claude/rules/r-code-conventions.md` with Python conventions

Rename existing file conceptually — actually **add** `.claude/rules/python-code-conventions.md` (keep r-code-conventions.md in case R is needed later for replication tables). The Python rule covers:

- Style: PEP 8, snake_case, type hints for public functions
- Reproducibility: `np.random.seed()` / `rng = np.random.default_rng(seed)`, relative paths, `requirements.txt` or `pyproject.toml`
- Numerical: prefer `numpy` broadcasting over loops, use `scipy.linalg` for matrix ops (not `numpy.linalg` for decompositions needing stability)
- Statistical: `statsmodels` for inference (not sklearn for SE), document estimator degrees of freedom
- Figures: `matplotlib` with explicit `fig, ax`, save as PDF/SVG for Beamer inclusion, consistent color palette
- Simulation: store results as `.npy` or `.pkl` for expensive runs, document DGP parameters in docstring
- Common pitfalls: float32 vs float64 precision, in-place mutation of arrays, sklearn pipelines leaking test data

---

### 4. Memory files

**`memory/user_profile.md`** (user type):
- Researcher targeting top-5 econ journals
- Wants structured, precise, rigorous collaboration
- Publication-ready visuals are non-negotiable
- Wants minimal repetition — decisions remembered
- Prefers more check-ins during early sessions to learn the workflow

**`memory/project_context.md`** (project):
- Project: Low-Rank Recovery and Inference for Sparsely Observed Treatment Effects
- Tools: LaTeX/Beamer + Python (NumPy/SciPy, statsmodels, scikit-learn, matplotlib/seaborn)
- Clean start as of 2026-03-30
- Target outlet: top-5 Economics journal
- Core problem: matrix recovery + valid inference (mean and SE) under low-rank structure with sparse column observations

**`MEMORY.md`** additions:
- Pointer to `memory/user_profile.md`
- Pointer to `memory/project_context.md`

---

### 5. Create `.claude/state/personal-memory.md`

This file is gitignored. Seed it with project-specific machine notes (Python path, any local setup).

---

## Verification

After implementation:
1. Open `CLAUDE.md` — confirm no `[YOUR ...]` placeholders remain (except institution)
2. Open `.claude/agents/domain-reviewer.md` — confirm 5 domain-specific lenses present
3. Open `.claude/rules/python-code-conventions.md` — confirm file exists and covers all 5 areas
4. Open `memory/user_profile.md` and `memory/project_context.md` — confirm both exist
5. Open `MEMORY.md` — confirm pointers added

---

## What Is NOT Changed

- `settings.json` — Python already in allowed commands; no changes needed
- All skills (compile-latex, commit, lit-review, etc.) — already generic, applicable as-is
- All hooks — already generic
- `r-code-conventions.md` — kept as-is (may be needed for replication appendix)
- Quality thresholds (80/90/95) — appropriate for this project's rigor standard

---

## Order of Execution

1. Update `CLAUDE.md`
2. Update `.claude/agents/domain-reviewer.md`
3. Create `.claude/rules/python-code-conventions.md`
4. Create memory files (`memory/user_profile.md`, `memory/project_context.md`)
5. Update `MEMORY.md` with pointers
6. Create `.claude/state/personal-memory.md`
7. Verify all files
