# Component Dependency Map – RideShareApp Platform

## Objective
Provide a structured format to map service-to-service dependencies, including data flows, latency sensitivity, and critical path indicators across the RideShareApp platform.

---

## Format Schema (YAML or Table-Based)

### Example YAML Format:

```yaml
ride-matcher:
  depends_on:
    - name: pricing-engine
      type: synchronous
      protocol: REST
      critical_path: true
      notes: Required to compute surge-adjusted fare before driver selection

    - name: driver-availability-store
      type: async
      protocol: Kafka
      critical_path: true
      notes: Provides zone-level driver inventory

    - name: trip-store
      type: async
      protocol: Kafka
      critical_path: false
      notes: Eventual consistency acceptable

pricing-engine:
  depends_on:
    - name: demand-forecaster
      type: synchronous
      protocol: gRPC
      critical_path: true
      notes: Needed for calculating real-time surge multipliers

eta-engine:
  depends_on:
    - name: routing-service
      type: synchronous
      protocol: REST
      critical_path: true
    - name: traffic-ingestor
      type: async
      protocol: Kafka
      critical_path: false
```

---

## Alternate Table Format

| Component         | Depends On             | Type        | Protocol | Critical Path | Notes                                      |
|------------------|------------------------|-------------|----------|----------------|--------------------------------------------|
| ride-matcher     | pricing-engine         | Sync        | REST     | Yes            | Surge fare used before match finalization  |
| ride-matcher     | driver-availability    | Async       | Kafka    | Yes            | Zone-level inventory                       |
| ride-matcher     | trip-store             | Async       | Kafka    | No             | For downstream analytics                   |
| pricing-engine   | demand-forecaster      | Sync        | gRPC     | Yes            | For real-time surge computation            |
| eta-engine       | routing-service        | Sync        | REST     | Yes            | Determines route time                      |
| eta-engine       | traffic-ingestor       | Async       | Kafka    | No             | For route time adjustments                 |

---

## Usage Recommendations
- All new services must register their dependencies using this format
- Include a `component-dependencies.yaml` in each repo
- Used during incident triage, SLO budgeting, and release coordination
- Graph visualizations auto-generated via backstage or mermaid diagrams

---

## Summary
This structured component dependency format ensures traceability, scalability impact analysis, and better platform resilience. It provides the foundation for runtime dependency maps, alert correlation, and chaos testing targeting.
