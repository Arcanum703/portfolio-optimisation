# Project 10 — Monte Carlo Portfolio Optimisation

University assignment (25% of total grade). One Jupyter notebook that builds the efficient frontier of 15 diversified large-cap US equities using:

1. **Monte Carlo** — 50,000 random portfolios, vectorised with NumPy.
2. **SciPy SLSQP** — gradient-based min-variance and max-Sharpe optimisation.
3. **Equal-weight (1/N)** — naive benchmark for comparison.
4. **Capital Market Line** — what happens when a risk-free asset is added.
5. **Rolling 1-year Sharpe** — stability check across COVID and the 2022 rate cycle.

---

## Prerequisites

You need **Python 3.10+** on PATH. Check with:

```powershell
python --version
```

If `python` is not found:

```powershell
winget install Python.Python.3.13
```

…then restart your terminal.

---

## How to launch (pick one path)

### Path A — VS Code (recommended)

1. Install the VS Code **Python** and **Jupyter** extensions.
2. Open this folder in VS Code: `File → Open Folder…` → pick `portfolio_optimisation`.
3. Open a terminal in VS Code (`` Ctrl+` ``) and install dependencies:
   ```powershell
   pip install -r requirements.txt
   ```
4. Click `portfolio_optimisation.ipynb` in the file tree.
5. Top-right of the notebook → **Select Kernel** → choose your Python 3.
6. Click **Run All** (▶▶ button at the top of the notebook).

Plots render inline below each code cell. Total runtime ≈ 30–60 seconds (the `yfinance` download is the slowest step).

### Path B — Terminal only (headless execution)

Run all cells without opening any UI; the executed notebook with all outputs is written back to the same file:

```powershell
pip install -r requirements.txt
python -m nbconvert --to notebook --execute portfolio_optimisation.ipynb --inplace
```

Then open `portfolio_optimisation.ipynb` in any notebook viewer (VS Code, JupyterLab, or even GitHub) to read it.

### Path C — JupyterLab in browser

```powershell
pip install -r requirements.txt
pip install jupyterlab
jupyter lab portfolio_optimisation.ipynb
```

---

## What gets installed

`requirements.txt` pins these:

| Package | Why |
|---|---|
| `yfinance` | Fetches 11 years of daily prices from Yahoo Finance |
| `pandas`, `numpy` | Data wrangling and vectorised math |
| `scipy` | SLSQP optimiser (`scipy.optimize.minimize`) |
| `matplotlib`, `seaborn` | All plots and the correlation heatmap |
| `nbformat`, `nbconvert`, `ipykernel` | Notebook execution and editing |

One-line install (no requirements file):

```powershell
pip install yfinance pandas numpy scipy matplotlib seaborn nbformat nbconvert ipykernel
```

---

## Files

| File | Purpose |
|---|---|
| `portfolio_optimisation.ipynb` | The deliverable — 31 cells, narrative + code + 7 plots |
| `requirements.txt` | Pinned dependencies |
| `figures/` | PNG copies of every plot (auto-saved during notebook run, useful for embedding in a written report) |
| `README.md` | This file |

---

## Notebook outline

| § | Section | Type |
|---|---|---|
| 1 | Introduction & methodology | markdown |
| 2 | Data acquisition (15 tickers, 2014–2024) | markdown + code |
| 3 | Returns, statistics, equal-weight baseline, correlation heatmap | code |
| 4 | Monte Carlo — 50,000 random portfolios | code |
| 5 | Efficient frontier — MC scatter, with **5b**: SLSQP-traced curve overlay | code |
| 6 | Gradient-based optimisation (SLSQP min-var + max-Sharpe) | code |
| 7 | Portfolio comparison table (5 portfolios) | code |
| 8 | Weight allocations (bar charts) + sector mix (pie charts) | code |
| 9 | Capital Market Line | code |
| 10 | Rolling 1-year Sharpe (stability check) | code |
| 11 | Conclusion | markdown |

---

## Reproducibility

`np.random.seed(42)` is set at the top of the imports cell, so every run produces identical numbers. Re-running on a different day will redownload the same fixed window (2014-01-01 → 2024-12-31).

---

## Troubleshooting

- **`python: command not found`** — install Python (`winget install Python.Python.3.13`) and restart the terminal.
- **`pip install` errors about long paths on Windows** — that's the `jupyter` meta-package failing on a deeply-nested static asset. Don't install plain `jupyter`; the packages in `requirements.txt` are the slim subset that works fine, and VS Code provides the UI.
- **`yfinance` returns empty data** — Yahoo occasionally rate-limits; rerun the download cell after a minute.
- **Notebook says "kernel not found"** in VS Code — click the kernel picker in the top right, then "Select Another Kernel" → "Python Environments" → your Python 3.x install.
