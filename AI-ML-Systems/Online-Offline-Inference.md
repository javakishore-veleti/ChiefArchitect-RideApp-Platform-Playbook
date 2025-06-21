# Online vs Offline Inference

## Objective
Define clear patterns, trade-offs, and best practices for supporting both online (real-time) and offline (batch) inference across the platform. This ensures consistent ML model behavior across contexts while optimizing for latency, scale, and cost.

---

## Inference Modes

### 🔹 Online Inference
- **Purpose**: Real-time predictions during transactional flows
- **Latency Target**: <100ms typical (P95)
- **Examples**:
  - Surge pricing multiplier at ride request
  - Fraud detection at payment submission
  - ETA prediction during dispatch

### 🔹 Offline Inference
- **Purpose**: Scheduled or triggered bulk predictions
- **Latency Target**: Minutes to hours
- **Examples**:
  - Weekly churn risk scoring
  - Nightly fare model refreshes
  - Large-scale segmentation for marketing

---

## Architecture Patterns

### ✅ Online Inference
- Stateless microservices exposed via REST/gRPC
- Low-latency feature stores (e.g., Redis, DuckDB, Cassandra)
- GPU/CPU autoscaling via KNative, Seldon, or custom controllers
- Integration with decision engines and feature caching

### ✅ Offline Inference
- Batched jobs via Airflow, Spark, or dbt
- Uses same feature pipeline logic as online for consistency
- Writes to data lake, analytics store, or downstream systems
- Versioned output and scheduled evaluation dashboards

---

## Deployment and Governance
| Topic | Online | Offline |
|-------|--------|---------|
| Deployment Frequency | Continuous or on-demand | Daily/weekly/scheduled |
| Model Versioning | Canary, blue/green, A/B | Timestamped batch tags |
| Rollback Strategy | Auto-rollback, feature switch | Reprocessing, overwrite logic |
| Monitoring | Latency, drift, request volume | Accuracy, coverage, cost-to-run |

---

## Feature Consistency
- Unified feature definitions across modes (shared metadata + logic)
- Use of transformation registry or dbt-based feature lineage
- Integration with feature store to avoid training-serving skew

---

## Success Metrics
- P95 latency of online inference
- % of online predictions reused from recent cache
- Accuracy parity between online and offline models
- Drift rate between batch and live feature inputs
- Cost-per-1000 inferences by mode

---

## Summary
Balancing online and offline inference strategies is critical to platform intelligence. By aligning feature logic, governance, and performance targets across both modes, the architecture ensures fast, reliable, and scalable ML deployment that serves real-time and batch use cases effectively.
