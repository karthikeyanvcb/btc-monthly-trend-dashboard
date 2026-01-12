# BTC Monthly Trend Dashboard

This project is a small personal exercise in data analysis.  It fetches daily Bitcoin data from the CoinGecko API, aggregates it to monthly frequency and displays simple charts to visualize trading volume, average price and returns.  The code is intentionally kept straightforward and beginner‑friendly.

## Data source

The dashboard pulls data using the CoinGecko API.  CoinGecko’s `market_chart` endpoint returns lists of prices, market caps and 24‑hour volumes for a coin over a specified number of days【191652890611206†L417-L425】.  The `days` query parameter accepts an integer or `max` to request all available data【797779859494858†L320-L329】.  In this project we request daily Bitcoin data denominated in U.S. dollars and let `days=max` return the full history.

## Project structure

```
BTC_Monthly_Trend_Dashboard/
├── data/        # cached raw data (CSV)
├── images/      # generated figures
├── notebook.ipynb  # analysis and visualizations
└── README.md    # this document
```

Running the notebook will create the `data/` and `images/` directories if they do not already exist.  The raw daily data are saved in `data/btc_daily_data.csv` so the API is only called once.

## What the notebook does

The Jupyter notebook performs the following steps:

1. **Fetch data** – Check if a cached CSV exists.  If not, send a GET request to CoinGecko’s `/coins/bitcoin/market_chart` endpoint with `vs_currency=usd`, `days=max` and `interval=daily` to obtain JSON lists of timestamps, prices and volumes.  Convert the lists into a tidy pandas DataFrame and save it to `data/btc_daily_data.csv`.
2. **Clean timestamps** – Convert the millisecond timestamps from the API to Python datetime objects.  Sort the data by date.
3. **Aggregate to monthly frequency** – Group the data by year‑month and compute:
   - *Monthly trading volume*: sum of daily volumes.
   - *Monthly average price*: mean of daily prices.
   - *Monthly return*: percentage change in the average price relative to the previous month.
   - *Three‑month rolling average*: a simple rolling mean of the monthly returns to smooth out volatility.
4. **Plot charts** using matplotlib:
   - **Monthly trading volume** – A bar chart showing the total trading volume in USD for each month.
   - **Monthly average price** – A line chart of the average price each month.
   - **Monthly returns with a 3‑month rolling average** – A bar chart for monthly returns with a line overlay for the rolling average.  This helps to see longer‑term momentum.
5. **Save figures** – Each chart is saved in the `images/` folder as a PNG file so they can be reused outside of the notebook.

## Interpreting the charts

The charts allow you to explore how Bitcoin trading activity and prices have changed over time.  Below are three simple observations based on widely reported milestones in Bitcoin’s history:

1. **Big bull runs stand out** – Bitcoin’s price climbed from under $1 000 to nearly $20 000 over 2017【704161183692111†L310-L318】 and later reached a new all‑time high of about $64 895 in April 2021【704161183692111†L388-L391】.  These bull runs show up in the dashboard as spikes in monthly trading volume and very high positive returns.
2. **Sharp drops and fast recoveries** – During the March 2020 market crash, Bitcoin briefly fell below $4 000 before rebounding and finishing the year around $30 000【704161183692111†L353-L365】.  In the monthly returns chart this appears as a large negative bar followed by a strong positive bar.
3. **More mainstream adoption** – By May 2025 Bitcoin was trading above $110 000 and the market seemed calmer, with lower volatility and volume compared with earlier years【704161183692111†L437-L443】.  The U.S. Securities and Exchange Commission’s approval of spot Bitcoin ETFs in January 2024【704161183692111†L416-L423】 helped drive renewed optimism.  The rolling average line smooths out these month‑to‑month swings and reveals the longer‑term upward trend.

Feel free to extend the analysis by experimenting with other cryptocurrencies, currencies or time windows.  The notebook is meant to serve as a starting point for personal data exploration rather than a polished product.  Keep the code clear and adapt it to your needs.