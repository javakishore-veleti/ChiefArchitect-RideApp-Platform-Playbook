# Event Storming – RideShareApp Platform

## Objective
Establish event storming as a collaborative modeling practice across business, product, design, and engineering teams within the RideShareApp Platform. This facilitates domain understanding, service boundary discovery, and alignment around core platform behaviors.

---

## 1. What is Event Storming?
Event storming is a visual, domain-driven design (DDD) technique to collaboratively map out business processes using domain events, commands, aggregates, and actors. It is especially useful for:
- Discovering bounded contexts
- Designing service ownership boundaries
- Defining event-driven workflows
- Aligning business and tech teams on process flows

---

## 2. Strategic Importance for RideShareApp
- **Complex, Interconnected Domains**: Booking, Pricing, Onboarding, Payments
- **Rapid Market Adaptation**: Evolve and reason about workflows visually
- **Event-Driven Platform**: Natural fit for Kafka and asynchronous microservices
- **Cross-Team Discovery**: Shared language between designers, PMs, and engineers

---

## 3. Key Roles in a Storming Workshop
| Role              | Responsibilities                                   |
|-------------------|----------------------------------------------------|
| Domain Experts     | Narrate business processes and edge cases         |
| Architects         | Capture event flow, identify aggregates & commands|
| Engineers          | Design potential service interfaces                |
| Designers/PMs      | Validate flows from UX and business intent        |

---

## 4. Artifact Components
- **Domain Events**: E.g., `TripBooked`, `FareEstimated`, `DriverLocationUpdated`
- **Commands**: E.g., `RequestBooking`, `ConfirmTripCompletion`
- **Aggregates**: E.g., `Trip`, `Rider`, `Driver`
- **Policies**: Trigger downstream actions, e.g., notify, match, update ETA
- **External Systems**: Payments, notifications, fleet APIs

---

## 5. Tooling & Templates
- Digital Board: Miro or Whimsical pre-configured event storming canvas
- Stickers & Tokens: Color-coded for events, commands, aggregates, policies
- Output Template: Markdown + PNG export of final model
- Event Catalog: Add discovered events to shared event-schema registry

---

## 6. Use Cases in RideShareApp
- Redesigning `Booking` domain during edge rollout
- Creating `Surge-Pricing` event flow with ML triggers
- Refactoring `Driver Onboarding` into micro-stages
- Defining `Notification Engine` policy triggers and escalation paths

---

## 7. Best Practices
- Focus on *domain events*, not screens or endpoints
- Use storytelling: walk through a single trip from rider perspective
- Embrace mess early; refine later with commands and aggregates
- Time-box modeling rounds and iterate over multiple sessions
- Document & archive boards in Git alongside domain artifacts

---

## 8. Roadmap
- **Q2**: Train 5 domain teams on event storming facilitation
- **Q3**: Mandate storming for all new domain API designs
- **Q4**: Integrate storming output into contract-first service templates

---

## Summary
Event storming fosters shared understanding and thoughtful service design in the RideShareApp Platform. As workflows become more event-driven and distributed, this practice helps ensure scalable boundaries, clearer contracts, and deeper collaboration across domains.