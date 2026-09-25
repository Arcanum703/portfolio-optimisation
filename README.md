# Project 10 — Monte Carlo Portfolio Optimisation

A single Jupyter notebook that builds the efficient frontier for a basket of 15 large-cap
US equities (AAPL, MSFT, NVDA, JPM, BAC, GS, JNJ, PFE, UNH, PG, KO, WMT, XOM, CAT, VZ)
using daily prices from 2014 to 2024. We find the frontier two ways, first by simulating
50,000 random portfolios with NumPy, then by solving for the minimum-variance and
maximum-Sharpe portfolios directly with SciPy's SLSQP optimiser, and check that the two
agree. Along the way we compare both against a naive equal-weight portfolio, add a
risk-free asset to get the Capital Market Line, and look at a rolling one-year Sharpe
ratio to see how each portfolio held up through COVID and the 2022 rate cycle.

This was written as a university assignment, but it should be readable on its own.

## Quick start

You need Python 3.10 or newer. From the repository root:

```bash
pip install -r requirements.txt
pip install jupyterlab
jupyter lab portfolio_optimisation.ipynb
```

JupyterLab itself isn't in `requirements.txt` (see Troubleshooting for why we keep that
file slim), hence the extra line. Once the notebook is open, run all cells. A full run
takes 30 to 60 seconds, most of it waiting on the `yfinance` download.

If you'd rather use VS Code, install the Python and Jupyter extensions, open the folder,
open the notebook, pick your Python 3 kernel and hit Run All. To execute everything
without opening any UI, run
`python -m nbconvert --to notebook --execute portfolio_optimisation.ipynb --inplace`,
which writes the outputs back into the notebook file.

## What's in the repo

- `portfolio_optimisation.ipynb` is the whole project: narrative, code and plots in one file.
- `requirements.txt` lists the dependencies. Prices come from Yahoo Finance via
  `yfinance`; pandas, NumPy and SciPy do the maths; matplotlib and seaborn do the plots;
  `nbformat`, `nbconvert` and `ipykernel` are there so the notebook can be executed
  from the command line.
- `figures/` holds a PNG of every plot. They're rewritten each time the notebook runs,
  which is handy if you want to drop them into a written report.

## What the notebook covers

The notebook starts with a short recap of mean-variance theory and the definitions it
uses (annualised return, volatility, Sharpe ratio with a 2% risk-free rate). It then
downloads adjusted closes for the 15 tickers, computes log returns, the annualised mean
and covariance, and a correlation heatmap, and sets up the equal-weight baseline.

The Monte Carlo section draws 50,000 weight vectors from a flat Dirichlet distribution so
they cover the simplex uniformly, and computes every portfolio's return and volatility in
a couple of vectorised operations. The scatter of those portfolios is the empirical
frontier; we then trace the exact frontier with 200 constrained SLSQP solves and overlay
it, and solve for the min-variance and max-Sharpe portfolios directly.

After that we put the Monte Carlo winners, the SLSQP optima and the equal-weight
portfolio side by side, plot their weights and sector mixes, add a risk-free asset to
derive the Capital Market Line, and finish with a 252-day rolling Sharpe ratio for each.

The main takeaways, in the notebook's own words: the minimum-variance portfolio leans on
consumer staples and healthcare (PG, JNJ, KO, WMT), the maximum-Sharpe portfolio
concentrates in NVDA, MSFT, UNH and WMT, SLSQP beats the Monte Carlo envelope by only a
few basis points, equal weighting gets you surprisingly close to either optimum, and the
2022 rate cycle is where the max-Sharpe portfolio suffers most while min-variance stays
the steadiest.

## Reproducibility

`np.random.seed(42)` is set in the imports cell, so the random portfolios are identical
from run to run, and the download window is fixed to 2014-01-01 to 2024-12-31, so
rerunning on a later date fetches the same data.

## Troubleshooting

If `python` isn't found on Windows, `winget install Python.Python.3.13` and reopen the
terminal. On macOS or Linux use your package manager or python.org.

If `pip install` fails with a long-path error on Windows, that's the `jupyter`
meta-package tripping over a deeply nested static asset. Don't install plain `jupyter`;
`requirements.txt` is the slim subset that works, and either JupyterLab or VS Code
provides the UI.

If `yfinance` comes back empty, Yahoo is probably rate-limiting you. Wait a minute and
rerun the download cell.

If VS Code says the kernel wasn't found, open the kernel picker in the top right, choose
"Select Another Kernel", then "Python Environments", then your Python 3 install.
