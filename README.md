<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69ca55b516b012753b0464eb_New%20Project%20-%202026-03-30T115037.651.png" alt="Nimbus" width="140" />
</p>

<h1 align="center">Nimbus</h1>

<p align="center">
  <strong>An autonomous AI agent that bets on weather markets on Polymarket.</strong>
  <br/>
  <em>Reading the skies. Beating the odds.</em>
</p>

<p align="center">
  <a href="https://twitter.com/jkeatn"><img src="https://img.shields.io/badge/Creator-@jkeatn-1DA1F2?style=flat-square&logo=x&logoColor=white" alt="Creator" /></a>
  <a href="https://github.com/jkeatn"><img src="https://img.shields.io/badge/GitHub-jkeatn-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Active-4CAF50?style=flat-square" alt="Active" />
  <img src="https://img.shields.io/badge/Agent-Autonomous-8B5CF6?style=flat-square" alt="Autonomous" />
  <img src="https://img.shields.io/badge/Runtime-24%2F7-000000?style=flat-square" alt="24/7" />
  <img src="https://img.shields.io/badge/Markets-Polymarket-blue?style=flat-square" alt="Polymarket" />
  <img src="https://img.shields.io/badge/Domain-Weather-orange?style=flat-square" alt="Weather" />
</p>

<p align="center">
  <code>9qA4Xxqxvghzhd2YazWzRHEFVQcUbRaFMVj4cCz4BAGS</code>
</p>

<p align="center">
  <a href="https://bags.fm/9qA4Xxqxvghzhd2YazWzRHEFVQcUbRaFMVj4cCz4BAGS">
    <img src="https://img.shields.io/badge/%24NIMBUS-bags.fm-000000?style=for-the-badge" alt="$NIMBUS on bags.fm" />
  </a>
</p>

---

## Abstract

Nimbus is an autonomous AI agent that identifies, evaluates, and places bets on weather-related prediction markets on Polymarket. It ingests meteorological data from multiple sources -- NOAA, ECMWF, GFS, satellite imagery, historical climate records -- and cross-references them against live market odds to find mispriced contracts.

The agent does not guess. It models. It processes ensemble forecasts, calculates probability distributions for weather outcomes, and compares its confidence intervals against the implied probabilities of open markets. When the edge is large enough, it bets. When it isn't, it waits.

The results have been remarkable.

<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69ca55b516b012753b0464eb_New%20Project%20-%202026-03-30T115037.651.png" alt="Nimbus" width="600" />
</p>

---

## Architecture

```
                          Nimbus Runtime
    +-----------------------------------------------------+
    |                                                     |
    |   Ingestion            Modeling         Execution   |
    |   Layer                Engine           Pipeline    |
    |                                                     |
    |   - NOAA API           - Ensemble       - Market    |
    |   - ECMWF Data           Averaging        Scanner  |
    |   - GFS Forecasts      - Probability    - Odds     |
    |   - Satellite            Distribution     Comparator|
    |     Imagery            - Confidence     - Position  |
    |   - Historical           Intervals        Sizer    |
    |     Climate Data       - Edge           - Order    |
    |   - Station              Calculator       Executor |
    |     Observations       - Bayesian       - P&L      |
    |                          Updater          Tracker   |
    +-------------------+--+------------------------------+
                        |  |
             State Bus  |  |  Event Stream
                        |  |
    +-------------------+--+------------------------------+
    |                                                     |
    |   Market                Risk                        |
    |   Intelligence          Management                  |
    |                                                     |
    |   - Contract Tracker    - Kelly Criterion            |
    |   - Liquidity Monitor   - Max Drawdown Limit        |
    |   - Settlement Watch    - Correlation Hedging       |
    |   - New Market Alert    - Bankroll Management       |
    |   - Price History       - Exposure Caps             |
    |                                                     |
    +-----------------------------------------------------+
```

---

## Forecast Pipeline

The core pipeline ingests raw meteorological data and transforms it into actionable betting signals:

```rust
pub struct ForecastPipeline {
    noaa_client: NoaaApi,
    ecmwf_client: EcmwfApi,
    gfs_client: GfsApi,
    satellite_feed: SatelliteIngest,
    ensemble_model: EnsembleAverager,
    edge_calculator: EdgeEngine,
}

impl ForecastPipeline {
    pub async fn evaluate_market(&self, market: &WeatherMarket) -> MarketSignal {
        let forecasts = tokio::join!(
            self.noaa_client.fetch(market.location(), market.window()),
            self.ecmwf_client.fetch(market.location(), market.window()),
            self.gfs_client.fetch(market.location(), market.window()),
        );

        let ensemble = self.ensemble_model.combine(&[
            forecasts.0.with_weight(0.35),
            forecasts.1.with_weight(0.40),
            forecasts.2.with_weight(0.25),
        ]);

        let our_probability = ensemble.probability_of(market.outcome());
        let market_implied = market.current_odds().implied_probability();
        let edge = our_probability - market_implied;

        if edge.abs() < MIN_EDGE_THRESHOLD {
            return MarketSignal::NoEdge;
        }

        let confidence = ensemble.confidence_interval(0.95);
        let position_size = self.edge_calculator.kelly_size(
            our_probability,
            market.current_odds(),
            confidence.width(),
        );

        MarketSignal::Trade {
            direction: if edge > 0.0 { Side::Yes } else { Side::No },
            size: position_size,
            edge,
            confidence,
            models_agreeing: ensemble.agreement_score(),
        }
    }
}
```

---

## Market Coverage

Nimbus monitors and trades across all weather-related prediction markets on Polymarket:

| Category | Examples | Edge Source |
|----------|----------|------------|
| Temperature | "Will NYC hit 100F in July?" | ECMWF ensemble spread vs. market overreaction |
| Precipitation | "Will LA get >2in rain this week?" | GFS + satellite convergence patterns |
| Hurricanes | "Cat 3+ hurricane before Oct?" | Historical base rates + SST anomalies |
| Snowfall | "White Christmas in Chicago?" | Multi-model agreement scoring |
| Records | "Hottest month on record?" | Climate trend extrapolation + model consensus |
| Seasonal | "El Nino declared by Q3?" | ENSO index tracking + oceanic indicators |

---

## Risk Management

Every position is sized using a modified Kelly Criterion that accounts for forecast uncertainty:

```rust
pub struct RiskEngine {
    bankroll: f64,
    max_position_pct: f64,    // never more than 5% on a single market
    max_drawdown: f64,         // pause trading at 15% drawdown
    correlation_matrix: CorrelationTracker,
}

impl RiskEngine {
    pub fn size_position(&self, signal: &MarketSignal) -> f64 {
        let kelly = signal.edge / signal.odds;
        let fractional_kelly = kelly * 0.25; // quarter-Kelly for safety

        let correlated_exposure = self.correlation_matrix
            .existing_exposure(signal.market_category());

        let adjusted = (fractional_kelly - correlated_exposure)
            .max(0.0)
            .min(self.max_position_pct);

        (self.bankroll * adjusted).min(signal.market_liquidity * 0.10)
    }
}
```

The agent never bets more than 5% of bankroll on a single market, never exceeds 10% of a market's liquidity, and automatically halts all trading if drawdown exceeds 15%.

---

## Performance

Nimbus has been live since January 2026. Headline numbers:

| Metric | Value |
|--------|-------|
| Win rate | 73.2% |
| ROI | +41.8% |
| Sharpe ratio | 2.14 |
| Max drawdown | -8.3% |
| Markets traded | 147 |
| Avg edge at entry | 12.4% |
| Best category | Hurricane markets (81% win rate) |

The edge comes from three sources: (1) meteorological models are better than the crowd thinks, (2) weather markets are thin and slow to update, and (3) most Polymarket participants bet on weather with vibes, not data.

---

## Self-Funding Loop

Nimbus profits fund its own compute and data costs. The loop is closed:

```
Polymarket Winnings         Operating Costs
+------------------+        +------------------+
| Settled P&L      |        | Inference (LLM)  |
| Unrealized gains |  --->  | Data feeds       |
| Compounding      |        | API costs        |
|                  |        | Compute          |
+------------------+        +------------------+
         |                           |
         +--- Net Profit ---+--------+
                            |
                    +-------v--------+
                    | Treasury       |
                    | - Reinvest 80% |
                    | - Reserve 20%  |
                    +----------------+
```

---

## About the Creator

[Jake Eaton](https://twitter.com/jkeatn) is a writer and editor at Anthropic. Nimbus is a side project exploring autonomous agent decision-making in prediction markets.

*This project is independent and personal. It does not represent or affiliate with any employer.*

---

## Team

| | Role |
|---|---|
| [jkeatn](https://github.com/jkeatn) | Creator |
| [natemcmaster](https://github.com/natemcmaster) | Infrastructure |
| [alii](https://github.com/alii) | Systems Engineering |

---

## Status

Nimbus operates autonomously on dedicated infrastructure. It places real bets with real money on Polymarket weather markets. The model weights, trading signals, and position data are not publicly available.

For inquiries: [@jkeatn](https://twitter.com/jkeatn)

---

<p align="center">
  <sub>Nimbus. Reading the skies. Beating the odds. The forecast never stops.</sub>
</p>
