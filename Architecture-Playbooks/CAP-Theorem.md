# CAP Theorem Tradeoffs – RideShareApp Platform

## Objective
Outline how the CAP Theorem (Consistency, Availability, Partition Tolerance) impacts system architecture decisions across the RideShareApp Platform. The goal is to make intentional, domain-driven tradeoffs for each service based on user expectations, data sensitivity, and operational risk.

---

## 1. Recap: CAP Theorem Essentials
The CAP Theorem states that in a distributed system, we can only guarantee two of the following three:
- **Consistency (C)**: Every read receives the most recent write
- **Availability (A)**: Every request receives a (non-error) response
- **Partition Tolerance (P)**: System continues to function despite network splits

Since network partitions are inevitable in RideShareApp’s geo-distributed cloud deployments, we must choose between **Consistency** and **Availability** for each domain.

---

## 2. Tradeoff Matrix – Domain by Domain
| Domain Area           | Chosen Tradeoff | Reasoning                                                                 |
|-----------------------|------------------|---------------------------------------------------------------------------|
| Trip Booking          | CP               | Strong consistency needed to avoid double bookings or overcommitment     |
| Rider ETA Display     | AP               | Stale data is tolerable; show best-effort ETA rather than error           |
| Driver Live Location  | AP               | Prioritize high availability; allow occasional inconsistency              |
| Fraud Rules Engine    | CP               | Deterministic results critical for audit and escalation                   |
| Driver Incentives     | CP               | Accurate, traceable bonuses are more important than uptime in edge cases |
| Notification Delivery | AP               | Prioritize availability; retries will handle partial failure              |
| Support Admin Tools   | CP               | Admin decisions require fresh data on driver/rider state                 |

---

## 3. Guiding Principles
- **Favor CP**:
  - When side effects involve money, trust, or legal reporting
  - For user-visible state transitions (e.g., trip status)

- **Favor AP**:
  - For non-critical observables (e.g., map positions, ETA estimates)
  - When systems are cache-first with eventual consistency tolerance

- **Service Contracts** must declare CAP assumptions in their design documentation and onboarding review

---

## 4. Implementation Techniques
- **CP-Oriented Systems**:
  - Use quorum writes/reads (e.g., in Cassandra or etcd)
  - Paxos/Raft-based consensus for state replication
  - Reject writes during partition if safety is at risk

- **AP-Oriented Systems**:
  - Use eventual consistency with conflict resolution strategies (e.g., LWW, CRDTs)
  - Cache invalidation policies and TTLs to guide state freshness
  - Accept stale reads during network delays

---

## 5. Governance
- CAP mode per service must be annotated in Backstage or service registry
- Incident reports must reference CAP expectations (e.g., "in AP mode, stale ETA served")
- Service SLOs should reflect mode (e.g., stricter latency for AP services)

---

## Summary
RideShareApp Platform adopts domain-aware CAP tradeoffs to balance user trust, operational resilience, and system scalability. From booking to incentives, each service clearly communicates its consistency/availability expectations to ensure both reliability and developer clarity.
