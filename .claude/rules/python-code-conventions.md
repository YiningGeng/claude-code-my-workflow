# Python Code Conventions

**Standard:** Senior researcher + publication quality. Code must be reproducible, numerically careful, and directly traceable to paper formulas.

---

## Reproducibility

- **Seed every RNG** at the top of each script or experiment function:
  ```python
  rng = np.random.default_rng(seed=42)   # preferred (NumPy 1.17+)
  # or: np.random.seed(42) for legacy compatibility
  ```
- **Relative paths only.** Never hardcode `/Users/...` paths. Use `pathlib.Path(__file__).parent`.
- **Pin dependencies** in `requirements.txt` or `pyproject.toml`. Include versions.
- **Store expensive simulation results** as `.npy` (arrays) or `.pkl` (dicts). Document parameters in the filename: `sim_n500_r3_reps1000.npy`.

---

## Style

- **PEP 8.** 88-char line limit (Black-compatible). Snake_case for variables and functions.
- **Type hints on public functions:**
  ```python
  def recover_matrix(Y: np.ndarray, r: int, lam: float) -> np.ndarray:
  ```
- **Docstring format** for functions used in papers:
  ```python
  def estimate_se(theta_hat, Y, r):
      """
      Compute standard error for theta_hat under low-rank model.

      Parameters
      ----------
      theta_hat : np.ndarray, shape (p,)
          Debiased estimator of the target parameter.
      Y : np.ndarray, shape (n, p)
          Observed (possibly sparse) outcome matrix.
      r : int
          Assumed rank of the true matrix.

      Returns
      -------
      se : np.ndarray, shape (p,)
          Estimated standard errors.

      Notes
      -----
      Implements Equation (X) from [Paper Section Y].
      """
  ```
- **Document DGP parameters** in the simulation function docstring, not just inline comments.

---

## Numerical Correctness

- **Use `scipy.linalg` for matrix decompositions** (not `numpy.linalg`) when numerical stability matters:
  - `scipy.linalg.svd(full_matrices=False)` for economy SVD
  - `scipy.linalg.solve` instead of `np.linalg.inv(A) @ b`
- **Default to float64.** Never use float32 for statistical computations — accumulation errors in large matrix products are non-trivial.
- **Check near-rank-deficiency.** Log a warning if the spectral gap `sigma_r / sigma_{r+1}` is below a threshold.
- **Prefer broadcasting over loops** for matrix operations. If a loop is unavoidable, add a comment explaining why.
- **Do not mutate arrays in-place** inside simulation loops — always copy before modifying:
  ```python
  Y_obs = Y.copy()
  Y_obs[mask] = np.nan
  ```

---

## Statistical Inference

- **Use `statsmodels` for inference**, not `sklearn`. sklearn estimators do not expose valid standard errors or covariance matrices.
- **Document degrees of freedom** for any t/F statistic: what's the effective sample size?
- **Distinguish estimand from estimator** in variable names:
  ```python
  theta_true   # population parameter (DGP)
  theta_hat    # estimator
  theta_db     # debiased estimator
  se_hat       # standard error estimate
  ci_lower, ci_upper  # confidence interval bounds
  ```
- **Always check coverage** in simulations: `np.mean((ci_lower <= theta_true) & (theta_true <= ci_upper))`.

---

## Figures (Publication Quality)

- **Always use explicit `fig, ax` pattern:**
  ```python
  fig, ax = plt.subplots(figsize=(5, 4))
  ax.plot(...)
  fig.savefig("Figures/sim_coverage.pdf", bbox_inches="tight", dpi=300)
  plt.close(fig)
  ```
- **Save as PDF or SVG** for Beamer inclusion. Never use PNG for paper figures.
- **Consistent color palette** across all figures in the paper. Define once at top of script:
  ```python
  COLORS = {"estimator": "#1f77b4", "oracle": "#d62728", "baseline": "#7f7f7f"}
  ```
- **Label axes with units and parameter values** (not just variable names).
- **Add grid lines sparingly.** Use `ax.grid(True, alpha=0.3)` if at all.

---

## Simulation Design

- **Match the DGP exactly** to the theoretical model on slides. Flag any simplifications.
- **Document the parameter grid** as a dict at the top of the simulation:
  ```python
  PARAMS = {
      "n_list": [100, 200, 500],
      "r": 3,
      "sigma": 1.0,
      "reps": 1000,
      "seed": 42,
  }
  ```
- **Report Monte Carlo SEs** for all simulation statistics: `se_mc = std(metric) / sqrt(reps)`.
- **Structure simulation output** as a DataFrame for easy plotting and tabulation.

---

## Common Pitfalls

| Pitfall | Wrong | Right |
|---------|-------|-------|
| SVD convention | `np.linalg.svd` (full by default) | `scipy.linalg.svd(full_matrices=False)` |
| Inference SEs | `sklearn` pipeline outputs | `statsmodels` with explicit covariance |
| In-place mutation | `Y[mask] = np.nan` inside loop | `Y_obs = Y.copy(); Y_obs[mask] = np.nan` |
| Hard-coded paths | `"/Users/me/project/data.npy"` | `Path(__file__).parent / "data.npy"` |
| Float precision | `np.float32` for accumulation | `np.float64` (default) |
| Seed state leakage | Global `np.random.seed` in function | `rng = np.random.default_rng(seed)` passed as arg |
