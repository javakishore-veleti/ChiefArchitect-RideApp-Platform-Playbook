# Event Schema Registry – RideShareApp Platform

## Objective
Ensure consistent, evolvable, and discoverable schemas for event-driven communication across RideShareApp services such as ride matching, surge pricing, driver telemetry, ETA forecasting, and loyalty operations, using a centralized schema registry.

---

## 1. Why a Schema Registry?
- Prevent downstream consumer breakage from schema changes in topics like `trip-booking-events` or `driver-location-stream`
- Enable forward and backward compatibility across evolving services such as `eta-engine`, `pricing-service`, `fleet-ops-dashboard`
- Improve discoverability of domain events like `ride.cancelled`, `surge.zone.updated`, `driver.eta.projection`
- Enforce schema validation and governance before publish/consume in Kafka

---

## 2. Supported Formats
- Avro (default for Kafka event streams across platform)
- Protobuf (for gRPC and long-lived ML pipelines)
- JSON Schema (used for webhooks, external partner events)

---

## 3. Registry Tools & Integration
- Registry: Confluent Schema Registry with multi-tenant namespaces
- CI/CD Integration:
  - All PRs modifying `/schemas/` run schema lint + compatibility test
  - Backward compatibility required by default (e.g., rider app consumers)
  - Automated promotion from dev → staging → prod registry environments
- Example topic structure:
  - `rider.ride.started`
  - `driver.status.update`
  - `pricing.dynamic.zone`

---

## 4. Schema Metadata Requirements
Each schema must provide:
- `event_name`, `version`, `contract_type` (event/command/notification)
- Description of business scenario (e.g., "Emitted when driver cancels trip pre-pickup")
- Domain owner (e.g., `Driver Engagement Platform`)
- Linked schema contract (e.g., `driver-cancelled-event.avsc`)
- PII Flags for fields (e.g., `location`, `user_id`, `phone`) with redaction tags

---

## 5. Versioning & Compatibility
- Semantic Versioning per schema: `v1.0.0`, `v1.1.0`, `v2.0.0`
- Compatible evolutions:
  - Add optional field (✅)
  - Add enum values (✅)
  - Remove required field or change type (❌)
- Registry policy: Allow backward compatibility for all consumer-facing schemas

---

## 6. Developer Workflow
1. Add schema file under `schemas/<domain>/<event>.avsc`
2. Add metadata in `schemas/<domain>/<event>.meta.yaml`
3. Validate via `rideapp-schema-check` CLI or CI Action
4. Review via domain architects + schema governance group
5. Merge triggers publish to dev registry
6. Registry promotion tied to service deployment tag

---

## 7. Best Practices for Schema Design
- Avoid deeply nested structures; prefer flat, extensible event shapes
- Use `correlation_id`, `ride_id`, and `trace_id` for cross-topic debugging
- Include `event_timestamp` and `event_version` as required fields
- Align schema names to domain language used in API and data models

---

## 8. Governance & Change Review
- Weekly schema review with Driver/Rider/Pricing architects
- Deprecation policy: Minimum 90-day support window for old versions
- Auto-alerts on incompatible changes via Slack and GitHub checks
- Schema lifecycle tracked in `schemas/CHANGELOG.md` with release tags

---

## Summary
The RideShareApp Schema Registry supports domain-aligned, resilient, and compliant event-driven architecture across our platform. It enables traceable changes, platform-wide discoverability, and contract testing that keeps real-time operations and ML models in sync as the system evolves.


# Event Schema Registry – RideShareApp Platform

## Objective
Ensure consistent, evolvable, and discoverable schemas for event-driven communication across RideShareApp services using a centralized event schema registry.

## 1. Why a Schema Registry?
- Prevent breaking changes in downstream consumers
- Enable backward/forward compatibility in event evolution
- Serve as documentation and governance tool for event-based interfaces
- Enforce schema validation before publishing

## 2. Supported Formats
- Avro (preferred for Kafka topics)
- Protobuf (for gRPC and hybrid pipelines)
- JSON Schema (for lightweight pub-sub or webhook systems)

## 3. Registry Tools & Integration
- Use Confluent Schema Registry or Apicurio for Kafka-native validation
- Integrate with CI/CD pipelines:
  - Schema linting on pull requests
  - Compatibility checks (BACKWARD/TRANSITIVE by default)
  - Auto-publish new versions to registry post-merge
- Registry organized by topic/domain:
  - `rider.ride.created`
  - `driver.location.updated`
  - `pricing.zone.surge.applied`

## 4. Schema Metadata
Each schema entry must include:
- `schema_id`
- `version`
- `created_by`
- `description`
- `source_team`
- `contract_type`: event, command, notification, log
- `retention_policy`
- `PII` flag (if personal data is embedded)

## 5. Versioning & Compatibility
- Use semantic versioning: `v1.0.0`, `v1.1.0`, `v2.0.0`
- Allowed schema evolutions:
  - Add optional field (allowed)
  - Remove required field (not allowed)
  - Change field type (not allowed)
- Document impact level (minor/major/incompatible)

## 6. Developer Workflow
1. Define schema in `/schemas/domain/topic-name.avsc`
2. Add `schema-metadata.yaml` with ownership info
3. Run `schema-validate` CLI or GitHub Action
4. Get schema reviewed by domain leads
5. Auto-publish to dev schema registry post-merge
6. Promote to staging/prod via release tags

## 7. Consumer Best Practices
- Always validate against registry schema before consuming
- Avoid tight coupling to exact schema shape
- Use durable subscriptions and schema evolution fallbacks
- Log unknown fields for backward compatibility checks

## 8. Governance & Auditing
- Bi-weekly schema review forum with architects and domain leads
- Enforce schema ownership and Slack escalation channel
- Auto-alerts on incompatible PRs, deprecated fields, and version drift
- Retain audit trail of schema changes and rollback versions

## Summary
The Event Schema Registry standardizes how RideShareApp services produce and consume events. It enables resilience, discoverability, and compliance for real-time, multi-tenant, event-driven communication across our global platform.
