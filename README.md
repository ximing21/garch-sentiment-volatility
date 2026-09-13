# Forecasting AAPL Volatility with GARCH and Financial-News Sentiment

This project tests whether lagged financial-news sentiment improves one-day-ahead
AAPL volatility forecasts beyond a baseline GARCH(1,1) model.

## Project overview

The analysis combines adjusted AAPL closing prices with dated financial-news
sentiment. To reduce look-ahead bias, each article is assigned to the next available
trading day. A baseline GARCH(1,1) model and a sentiment-augmented GARCH model are then
evaluated over the same 2018–2020 test period using an expanding-window forecast.

The augmented mean equation is

$$r_t = \mu + \gamma S_t + \epsilon_t,$$

while conditional variance follows

$$\sigma_t^2 = \omega + \alpha\epsilon_{t-1}^2 + \beta\sigma_{t-1}^2.$$

## Results

| Model | MSE | QLIKE |
| --- | ---: | ---: |
| Baseline GARCH | 1.59523495e-06 | -7.051042 |
| GARCH + sentiment | 1.59382242e-06* | -7.051395 |

The sentiment model produced a marginally lower QLIKE loss, but the improvement was
negligible. In this experiment, a simple lagged daily-average sentiment feature did
not add meaningful predictive value beyond the volatility persistence already captured
by GARCH(1,1).

## Methods

- Download adjusted AAPL prices and calculate daily log returns.
- Split observations chronologically into training and test periods.
- Produce one-day-ahead forecasts using an expanding window.
- Align news to the next trading day and aggregate sentiment by target date.
- Compare models using variance MSE and QLIKE loss; lower values are better.

## Repository structure

```text
.
├── README.md
├── garch_sentiment_analysis.ipynb
├── requirements.txt
└── data/
    └── tech_sentiment_progress.csv
```

## Running the notebook

1. Clone or download this repository.
2. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Start Jupyter from the repository root and run
   `garch_sentiment_analysis.ipynb` from top to bottom.

The market prices are downloaded when the notebook runs. The sentiment CSV is read
from `data/tech_sentiment_progress.csv`; no Google Drive mount is required.

## Limitations

- Exact publication timestamps were unavailable, so news was conservatively aligned
  to the next trading day.
- Daily averaging can obscure extreme sentiment and disagreement across articles.
- Squared daily returns are noisy proxies for latent daily variance.
- Results are limited to AAPL and the selected historical period.

## Data sources

- AAPL prices are downloaded using `yfinance`.
- Financial headlines come from Miguel Aenlle's Kaggle dataset,
  [Daily Financial News for 6000+ Stocks](https://www.kaggle.com/datasets/miguelaenlle/massive-stock-news-analysis-db-for-nlpbacktests),
  which is listed under the CC0: Public Domain license. The included CSV is the
  project-specific processed subset used in this analysis.
