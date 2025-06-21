# Logging Standards – RideShareApp Platform

## Objective
Define consistent, structured, and actionable logging practices across all services in the RideShareApp Platform to support observability, debugging, incident response, and auditability.

---

## 1. Logging Format & Structure
- Use **JSON-formatted structured logs** across all services
- Standard fields:
  - `timestamp` (ISO 8601)
  - `level` (DEBUG, INFO, WARN, ERROR)
  - `service` (e.g., driver-matching)
  - `env` (dev/staging/prod)
  - `request_id`, `trace_id`, `span_id`
  - `tenant_id`, `user_id` (if applicable)
  - `msg` (human-readable summary)
- Log example:
```json
{
  "timestamp": "2025-06-21T14:33:10Z",
  "level": "INFO",
  "service": "pricing-engine",
  "env": "prod",
  "request_id": "abc-123",
  "user_id": "rider-987",
  "msg": "Applied surge multiplier: 1.4"
}
```

---

## 2. Logging Levels
- `DEBUG`: Detailed developer info (disabled in production)
- `INFO`: Key lifecycle events (e.g., API request received, external call made)
- `WARN`: Retryable failures, degraded dependencies
- `ERROR`: Unexpected exceptions or failed outcomes
- `FATAL`: Service not recoverable without manual intervention

---

## 3. Sensitive Data & PII
- **Never log passwords, card numbers, access tokens, or private keys**
- Mask partial identifiers where necessary (e.g., phone: `xxx-xxx-4567`)
- Use central redaction middleware or pre-serialization sanitization

---

## 4. Correlation & Traceability
- Propagate `request_id` and `trace_id` across all internal hops (gRPC, Kafka, REST)
- Link logs with traces (via OpenTelemetry IDs)
- Include context from middleware: HTTP headers, user agent, geo IP if useful

---

## 5. Log Aggregation & Routing
- All logs are routed to centralized sink (e.g., Datadog, ELK, CloudWatch)
- Use log levels as filter criteria in dashboards
- Create routing rules per service and environment

---

## 6. Log Retention & Compliance
- Retention policy:
  - 7 days for DEBUG/INFO
  - 30 days for ERROR/FATAL
  - 90+ days for compliance-sensitive services (e.g., billing, auth)
- Retention settings reviewed quarterly with infosec and legal

---

## 7. Best Practices
- One log line per logical event
- Avoid multiline stack traces (flatten and serialize)
- Log before and after retries
- Add tags for feature flags and deployment versions
- Avoid logging in tight loops or per-iteration

---

## 8. Tools & Libraries
- Preferred logging libraries:
  - Java: Logback + SLF4J with JSON encoder
  - Go: Zap or Zerolog
  - Python: Structlog
  - Node: Pino or Winston
- Provide service-side middleware via `rideapp-logging-sdk`

---

## Summary
Consistent structured logging helps developers trace issues faster, enables automated monitoring, and meets audit requirements in a regulated, multi-tenant environment like RideShareApp. Follow these standards to ensure every log is meaningful, secure, and actionable.
