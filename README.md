# AXIOM.MARKETS — Stock Intelligence Dashboard

A fully client-side stock analysis tool that fetches real-time price data from Yahoo Finance and runs proven technical analysis entirely in the browser. No API keys, no backend, no cost.

**[Live Demo →](https://awouellette.github.io/Stock-Analyzer/)**

---

## Features

- **Live price data** pulled directly from Yahoo Finance
- **7 technical indicators** calculated in real-time from 1 year of daily price history
  - RSI (14-day)
  - Simple Moving Averages (20, 50, 200-day)
  - Exponential Moving Averages (12, 26-day)
  - MACD
  - Bollinger Bands (20-day, 2σ)
  - Average True Range (ATR)
  - Annualized Volatility
- **Fundamental data** — P/E, Forward P/E, EPS, Market Cap, Beta, Dividend Yield, Profit Margin
- **Weighted scoring engine** that produces a STRONG BUY → STRONG SELL recommendation with a confidence percentage
- **Dynamic price targets** — Support, Price Target, and Stop Loss calculated from ATR
- **Risk factor analysis** generated from actual indicator values
- Works on any device — fully responsive

---

## How It Works

### Data
Price data is fetched from the Yahoo Finance public JSON API via [corsproxy.io](https://corsproxy.io), which handles the browser CORS restriction. No authentication or API key is required.

### Analysis Engine
All indicators are calculated in JavaScript from the raw OHLCV (open/high/low/close/volume) price data:

| Indicator | Method | Signal |
|---|---|---|
| RSI (14) | Wilder smoothing | < 30 bullish, > 70 bearish |
| SMA 50-day | Simple average | Price above = bullish |
| SMA 200-day | Simple average | Price above = bullish |
| MACD | EMA(12) − EMA(26) | Positive = bullish |
| Bollinger Bands | 20-day SMA ± 2σ | Below lower band = bullish |
| 1M Momentum | 21-day return | > +5% = bullish |
| P/E Ratio | From fundamentals | < 15 = bullish, > 40 = bearish |

### Scoring
Each signal is scored from −2 (strong bear) to +2 (strong bull) and multiplied by its weight:

| Signal | Weight |
|---|---|
| SMA 200-day | 3 |
| RSI | 2 |
| SMA 50-day | 2 |
| MACD | 2 |
| 1M Momentum | 2 |
| Bollinger Bands | 1 |
| P/E Ratio | 1 |

The weighted average produces a score from −1 to +1:

| Score | Recommendation |
|---|---|
| > +0.55 | STRONG BUY |
| +0.20 to +0.55 | BUY |
| −0.20 to +0.20 | HOLD |
| −0.55 to −0.20 | SELL |
| < −0.55 | STRONG SELL |

Confidence is derived from how far the score is from zero — strongly agreeing signals produce higher confidence.

---

## Deployment

### GitHub Pages (recommended)

1. Fork or clone this repository
2. Rename `stock-analyzer.html` to `index.html`
3. Push to your GitHub repository
4. Go to **Settings → Pages → Deploy from branch → main / root**
5. Your site will be live at `https://your-username.github.io/repo-name`

No build step, no configuration, no environment variables needed.

### Local Development

Open `index.html` directly in a browser — it works without a local server since all requests go to external APIs.

> **Note:** Some browsers block mixed-content or CORS requests when opening files via `file://`. If you see errors locally, serve it with a simple static server:
> ```bash
> npx serve .
> # or
> python3 -m http.server 3000
> ```

---

## Tech Stack

- Vanilla HTML / CSS / JavaScript — zero dependencies, zero build tooling
- [Yahoo Finance](https://finance.yahoo.com) — free public price data
- [corsproxy.io](https://corsproxy.io) — open CORS proxy for browser-side Yahoo Finance requests
- [Google Fonts](https://fonts.google.com) — Syne + JetBrains Mono

---

## Limitations

- **Price data latency** — Yahoo Finance data may be delayed 15 minutes during market hours
- **CORS proxy dependency** — if corsproxy.io is unavailable, data fetching will fail
- **No streaming data** — prices are fetched on demand, not pushed in real-time
- **Technical analysis only** — no earnings estimates, sentiment, insider activity, or news signals
- **Not financial advice** — this tool is for informational and educational purposes only

---

## Disclaimer

This application is for **informational and educational purposes only**. Nothing in this tool constitutes financial advice, investment advice, or a recommendation to buy or sell any security. Always conduct your own research and consult a qualified financial advisor before making investment decisions.

---

## License

MIT — free to use, modify, and distribute.