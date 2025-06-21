# Business Capability Mapping

## Objective
Establish a clear and shared understanding of the platform’s business capabilities and how they align with product strategy, customer outcomes, and architectural domains. This enables prioritization, ownership clarity, and evolutionary scaling.

---

## What is a Business Capability?
A business capability is **what** the business does—not how. It represents a stable, customer-facing or internally critical function that the organization must perform to deliver value.

Examples:
- Booking Management
- Provider Allocation
- Customer Support
- Partner Onboarding
- Incentives & Promotions

---

## Benefits of Capability Mapping
- Align engineering teams and domain models with business strategy
- Make investment decisions and resource allocation more objective
- Identify duplications, capability gaps, and modernization opportunities
- Create a shared vocabulary between product, engineering, and leadership

---

## Capability-to-Domain Mapping Table

| Business Capability | Description | Primary Domain Owner | Supporting Systems |
|---------------------|-------------|-----------------------|---------------------|
| Booking Management | Schedule, cancel, and manage rides | Ride Services Team | Ride Service, Fare Estimation, Notifications |
| Provider Allocation | Match riders with drivers/providers | Allocation Services Team | Geo Router, Location Tracker, Ratings Engine |
| Partner Onboarding | Register and configure fleets/dealers | Partner Ops Team | Partner Portal, SLA Configurator, Admin Console |
| Payments & Billing | Handle fare, incentives, settlements | Billing Team | Payment Gateway, Ledger, Payout Scheduler |
| Loyalty & Incentives | Reward frequent usage, referral bonus | Growth Team | Campaign Manager, Rewards Engine |
| Customer Support | Handle escalations, disputes, help tickets | Support Engineering | Admin Dashboard, Ticketing System, Notifications |
| Ratings & Feedback | Capture user/provider reviews | Experience Team | Review API, Fraud Analyzer, Dashboard Widgets |

---

## Capability Maturity Model (Sample)
| Maturity Level | Criteria |
|----------------|----------|
| Level 1 – Initial | Manual, no SLAs, tribal knowledge |
| Level 2 – Defined | Clear scope, partial automation, team assigned |
| Level 3 – Standardized | APIs, SLAs, monitoring, reusable components |
| Level 4 – Scalable | Multi-region ready, extensible, A/B tested |
| Level 5 – Optimized | Autonomous, predictive, AI-assisted |

> Use the model to audit each capability annually and guide modernization efforts.

---

## Strategic Use Cases
- Portfolio Planning and Capability Gap Analysis
- Budget justification for platform initiatives
- Risk assessment across business-critical capabilities
- Onboarding tool for new PMs and architects

---

## Summary
Business capability mapping connects platform services with what matters most: business value. It clarifies ownership, accelerates planning, and creates a transparent structure for cross-functional decision-making and platform evolution.
