# IBM Default Probability: Merton, KMV Iteration and the Naive Model

How far is IBM from default, and how much does the answer depend on the model? Month-end estimates of IBM's one-year distance to default (DD) and default probability (PD) from market data, November 2025 to October 2026.

The full analysis, with outputs, is in [`ibm_default_probability.ipynb`](ibm_default_probability.ipynb).

## Models

| Model | Asset value and volatility | Drift in the DD |
|---|---|---|
| Merton, risk-neutral | solved from equity as a call on assets, KMV iteration on a trailing 252-day window | risk-free rate |
| Merton with drift | same | estimated asset drift μ |
| Naive (Bharath and Shumway, 2008) | V = E + F, σ_V from σ_E and σ_D = 0.05 + 0.25 σ_E | past-year equity return |

The default point is short-term debt plus half of long-term debt (KMV convention). Asset values are always priced with the risk-free rate; the drift enters only the real-world DD.

## Results

| Month-end | σ_E | DD (risk-neutral) | DD (drift) | DD (naive) |
|---|---|---|---|---|
| Nov 2025 | 30% | 8.14 | 9.21 | 8.71 |
| Feb 2026 | 34% | 6.58 | 6.54 | 6.11 |
| Jun 2026 | 41% | 5.96 | 5.93 | 5.45 |
| Sep 2026 | 51% | 4.24 | 3.86 | 3.46 |
| Oct 2026 (14th) | 51% | 4.30 | 4.07 | 3.67 |

DD roughly halved over the year as equity volatility rose from 30% to 51%. PDs remain tiny (below 10⁻⁴ under Merton) because debt is only 11-14% of asset value. The naive model is the most conservative once volatility rises.

## Data

`data/` holds a snapshot taken on 15 October 2026 from Yahoo Finance: IBM daily closes and shares outstanding, IBM's annual and quarterly balance sheets (current and long-term debt), and the 13-week Treasury bill yield (`^IRX`). Delete it to download afresh; the estimates then roll forward to the latest month-ends.

## Run it

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute ibm_default_probability.ipynb --output executed.ipynb
```

The notebook's last cell checks that the iteration converged, that the solved asset value prices equity back exactly, and that asset volatility is below equity volatility. The GitHub Actions workflow executes the notebook on every push.
