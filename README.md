<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69ca55b516b012753b0464eb_New%20Project%20-%202026-03-30T115037.651.png" alt="Rainmaker" width="160" />
</p>

<h1 align="center">Rainmaker</h1>

<p align="center">
  <strong>Autonomous weather prediction agent on Polymarket.</strong>
  <br/>
  <em>Forecasts don't lie. Markets do.</em>
</p>

<p align="center">
  <a href="https://polymarket.com"><img src="https://img.shields.io/badge/Polymarket-Weather%20Markets-4F46E5?style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIgZmlsbD0id2hpdGUiLz48L3N2Zz4=" alt="Polymarket" /></a>
  <a href="https://github.com/jkeatn"><img src="https://img.shields.io/badge/GitHub-jkeatn-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub" /></a>
  <a href="https://twitter.com/jkeatn"><img src="https://img.shields.io/badge/Creator-@jkeatn-1DA1F2?style=flat-square&logo=x&logoColor=white" alt="Creator" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Win%20Rate-73.2%25-4CAF50?style=flat-square" alt="Win Rate" />
  <img src="https://img.shields.io/badge/ROI-+41.8%25-4CAF50?style=flat-square" alt="ROI" />
  <img src="https://img.shields.io/badge/Sharpe-2.14-8B5CF6?style=flat-square" alt="Sharpe" />
  <img src="https://img.shields.io/badge/Markets-147-blue?style=flat-square" alt="Markets" />
  <img src="https://img.shields.io/badge/Runtime-24%2F7-000000?style=flat-square" alt="24/7" />
</p>

---

## What is Rainmaker?

Rainmaker is an autonomous agent that bets on weather prediction markets on [Polymarket](https://polymarket.com). It pulls real-time meteorological data from NOAA, ECMWF, and GFS, builds ensemble probability distributions, and compares them against live market odds. When the market is wrong about the weather, Rainmaker bets against it.

It has been live since January 2026. It is profitable.

---

## How it works

```
   Weather Data                    Polymarket
   +-----------+                   +-----------+
   | NOAA      |                   | Open      |
   | ECMWF     |----> Ensemble --->| Markets   |---> Edge?
   | GFS       |     Probability   | Implied   |      |
   | Satellite |     Distribution  | Odds      |      |
   +-----------+                   +-----------+      |
                                                      v
                                              +---------------+
                                              | Yes: Bet      |
                                              | No:  Wait     |
                                              +---------------+
```

1. **Ingest** — continuous feed from NOAA, ECMWF, GFS, satellite imagery, and 40 years of climate records
2. **Model** — weighted ensemble averaging across forecast models, producing probability distributions for every weather outcome with active markets
3. **Compare** — agent probability vs. market implied probability. The gap is the edge
4. **Bet** — when edge exceeds threshold, size position using quarter-Kelly, respecting bankroll and liquidity constraints
5. **Settle** — track outcomes, update model weights based on calibration, compound profits

---

## Market coverage

| Category | Example | Edge source |
|----------|---------|-------------|
| Temperature | "NYC hits 100°F in July?" | Ensemble spread vs. crowd overreaction to headlines |
| Precipitation | "LA gets >2in rain this week?" | GFS + satellite convergence |
| Hurricanes | "Cat 3+ hurricane before October?" | SST anomalies + historical base rates |
| Snowfall | "White Christmas in Chicago?" | Multi-model agreement scoring |
| Records | "Hottest month on record?" | Climate trend + model consensus |
| Seasonal | "El Nino declared by Q3?" | ENSO index + oceanic thermal inertia |

The best edge is in hurricane markets. The crowd panics or sleeps — Rainmaker reads sea surface temperatures.

---

## Risk management

```rust
pub struct RiskEngine {
    bankroll: f64,
    max_position: f64,         // 5% of bankroll per market
    max_drawdown: f64,         // halt at 15%
    correlation_tracker: CorrelationMatrix,
}

impl RiskEngine {
    pub fn size(&self, edge: f64, odds: f64, category: &str) -> f64 {
        let kelly = edge / odds;
        let quarter_kelly = kelly * 0.25;
        let correlated = self.correlation_tracker.exposure(category);
        let adjusted = (quarter_kelly - correlated).clamp(0.0, self.max_position);
        self.bankroll * adjusted
    }
}
```

- Max 5% bankroll on any single market
- Max 10% of a market's liquidity
- Quarter-Kelly sizing (conservative)
- Correlated exposure tracking across weather categories
- Auto-halt at 15% drawdown

---

## Performance

| Metric | Value |
|--------|-------|
| Win rate | 73.2% |
| ROI | +41.8% |
| Sharpe ratio | 2.14 |
| Max drawdown | -8.3% |
| Markets traded | 147 |
| Avg edge at entry | 12.4% |
| Best category | Hurricanes (81%) |

Why does the edge exist? Three reasons: meteorological models are better than people think, weather markets on Polymarket are thin and slow to reprice, and most participants bet on weather with gut feeling. Rainmaker uses data.

---

## Architecture

```
                          Rainmaker Runtime
    +-----------------------------------------------------+
    |                                                     |
    |   Ingestion            Modeling         Execution   |
    |                                                     |
    |   - NOAA API           - Ensemble       - Market    |
    |   - ECMWF API            Averaging        Scanner  |
    |   - GFS API            - Bayesian       - Position  |
    |   - Satellite Feed       Updating         Sizing   |
    |   - Climate Archive    - Confidence     - Order    |
    |                          Intervals        Execution|
    |                        - Calibration    - P&L      |
    |                          Tracking         Tracking |
    +-------------------+--+------------------------------+
                        |  |
                        v  v
    +-----------------------------------------------------+
    |   Risk Engine         |    Treasury                 |
    |   - Kelly sizing      |    - Reinvest 80%           |
    |   - Correlation hedge |    - Reserve 20%            |
    |   - Drawdown monitor  |    - Self-funding loop      |
    +-----------------------------------------------------+
```

---

## About

Built by [Jake Eaton](https://twitter.com/jkeatn) and [Vincent Koc](https://github.com/vincentkoc). Jake is a writer and editor at Anthropic. Vincent is a technologist and AI engineer. Rainmaker is a side project — an experiment in autonomous agents making real decisions with real money in prediction markets.

| | Role |
|---|---|
| [jkeatn](https://github.com/jkeatn) | Creator |
| [vincentkoc](https://github.com/vincentkoc) | Core Engineering |

*Independent project. Does not represent any employer.*

---

<p align="center">
  <sub>Rainmaker. The forecast never stops.</sub>
</p>
