# ETA and Demand Forecasting

## Objective
Implement robust machine learning systems for accurate Estimated Time of Arrival (ETA) and Demand Forecasting to improve rider/driver experience, optimize dispatching, and enable operational planning at scale.

---

## Strategic Goals
- Provide accurate, real-time ETA for riders and drivers
- Predict demand surges and supply gaps in advance
- Support dynamic pricing, fleet allocation, and staffing decisions
- Improve platform reliability and user trust

---

## ETA Prediction

### Key Factors Considered
- Real-time traffic data and road closures
- GPS drift and signal quality
- Historical trip duration per zone or route
- Driver behavior and fleet type (bike, car, EV)

### Model Architecture
- Gradient Boosted Trees or Deep Neural Networks
- Real-time feature extraction from map services and telemetry
- Feedback loop from post-trip actuals for model correction

### Inference Path
- gRPC endpoint invoked during search/match/confirm phases
- Caching near-duplicate queries via vector similarity or zone-hash

---

## Demand Forecasting

### Forecast Horizons
| Horizon | Use Case |
|---------|----------|
| 5–15 mins | Real-time surge triggers, heatmaps |
| 1–2 hrs | Near-term dispatch guidance |
| 24–72 hrs | Driver incentives, fleet rebalancing |
| 7–30 days | Strategic planning, ops staffing, promo design |

### Model Inputs
- Historical trip logs and heatmaps
- Weather, calendar (holidays/events), economic activity
- Search-to-book ratio and cancellations
- Region-specific seasonal trends

### Model Types
- Time series ensembles (ARIMA, Prophet, LSTM)
- Graph Neural Networks for zone adjacency effects
- Multivariate regressors with external factors

---

## Serving Architecture
- Online inference via shared ML service
- Pre-warmed models per city/zone
- Feature store integration for streaming updates
- Nightly retraining for short-horizon models

---

## Evaluation Metrics
| Metric | Description |
|--------|-------------|
| MAE / RMSE | Accuracy of ETA or demand prediction |
| Hit Rate | % predictions within X% of actual outcome |
| Coverage | % of zones or times with available forecast |
| Drift | Change in performance across regions or seasons |
| Uptime | Serving reliability of prediction endpoints |

---

## Impact and Outcomes
- Reduced cancellations due to ETA mismatches
- Faster allocation and better asset utilization
- Cost savings on under/over-supply mitigation
- Higher NPS from accurate rider expectations

---

## Summary
ETA and demand forecasting models are foundational AI assets for operational efficiency and user satisfaction. With real-time data, accurate predictions, and scalable serving, the platform can deliver seamless experiences and proactive supply-demand balancing across regions.
