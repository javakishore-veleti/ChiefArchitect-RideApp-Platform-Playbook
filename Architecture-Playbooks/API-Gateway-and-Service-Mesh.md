# API Gateway and Service Mesh Patterns – RideShareApp Platform

## Objective
Define the architecture, patterns, and operational standards for API Gateways and Service Mesh within the RideShareApp Platform to ensure secure, observable, and scalable communication across internal and external services.

---

## 1. Purpose
- **API Gateway**: Edge-facing traffic management, routing, and policy enforcement for Rider/Driver/Partner APIs
- **Service Mesh**: Internal service-to-service traffic observability, security (mTLS), and network reliability

---

## 2. API Gateway Architecture
| Component            | Role                                                                 |
|----------------------|----------------------------------------------------------------------|
| Envoy + Istio GW     | Ingress gateway for Rider/Driver/Partner APIs                        |
| Authentication       | OAuth2 for rider/driver auth, JWT for partner apps                   |
| Rate Limiting        | Per-client quotas on endpoints like `trip/book`, `pricing/query`     |
| IP Filtering         | Partner APIs restricted by allowlists                                |
| Custom Domains       | vanity URLs per tenant (e.g., `partner1.api.rideshareapp.com`)       |
| Usage Analytics      | Exported to BigQuery and Looker dashboards                           |

---

## 3. Internal Service Mesh Design
- **Mesh Implementation**: Istio with sidecar proxies injected into all GKE workloads
- **Security**:
  - mTLS enabled globally (STRICT mode)
  - PeerAuth policies between Booking, Pricing, and ML model services
- **Traffic Policies**:
  - Retries and circuit breakers for Pricing and ETA services
  - Weighted routing for canary deploys
- **Observability**:
  - Prometheus metrics collection from Envoy sidecars
  - Distributed tracing with Jaeger and B3 headers
  - SLO dashboards auto-generated in Grafana per namespace

---

## 4. Gateway-to-Mesh Flow
```mermaid
graph TD
    A[Rider Mobile App] -->|HTTPS| B[API Gateway (Envoy)]
    B -->|JWT AuthN + Rate Limit| C[Booking Service in Mesh]
    C --> D[Trip Allocation]
    C --> E[Pricing Service]
```

---

## 5. Design Patterns
### Pattern 1: BFF (Backend-for-Frontend)
- Used for Rider and Driver apps to aggregate trip, pricing, and promotions into mobile-optimized JSON

### Pattern 2: Token Translation Proxy
- Gateway handles OAuth → internal JWT token mapping
- Avoids leaking internal identity tokens to frontend clients

### Pattern 3: Mesh Gateway Split
- Use `istio-ingress` for external traffic, internal east-west traffic controlled by mesh-only gateways

### Pattern 4: Multi-Tenant Isolation
- Envoy filters per tenant with dynamic route rules and logging metadata

---

## 6. Governance
- APIs must be published with OpenAPI spec in Backstage catalog
- Partner APIs go through Platform Security review and quota modeling
- All mesh traffic is tagged with `x-context-id` headers for traceability
- Canary traffic rollout policies must be declared in Helm/Argo manifests

---

## Summary
RideShareApp Platform's Gateway and Mesh strategy is designed for security, agility, and insight. While API Gateways manage external contracts and rate-limits, Service Mesh patterns ensure resilient, observable internal communication—together forming a unified traffic and policy layer.
