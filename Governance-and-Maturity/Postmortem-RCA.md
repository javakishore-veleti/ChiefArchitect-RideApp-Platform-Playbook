# Postmortems and RCA Templates – RideShareApp Platform

## Objective
Establish a structured, blameless postmortem and RCA (Root Cause Analysis) framework for the RideShareApp Platform to extract learnings from incidents, improve systemic reliability, and close feedback loops between teams.

---

## 1. Principles
- **Blameless Culture**: Focus on systemic contributors and safeguards, not individual mistakes.
- **Action-Oriented**: Each RCA results in specific remediation owners and tracked follow-ups.
- **Transparency**: All Tier 1 incidents are shared in the #platform-postmortems Slack channel and stored in a searchable repo.
- **Cross-functional View**: Include inputs from Engineering, SRE, ML Ops, and Customer Support.

---

## 2. When to Trigger a Postmortem
- Tier 1 outage (e.g. trip booking down, rider matching failure, P0 SLO breach)
- Unexpected service regression or rollback in production
- Security or compliance breach (e.g. overexposed driver data, retention policy violation)
- Repeated alerts or noise indicating design gaps or operational debt

---

## 3. Postmortem Template Structure (RCA Document)
```markdown
# Incident Title

## Summary
- Date & Time:
- Duration:
- Impact:
- Teams Involved:

## Timeline (UTC)
| Time       | Event                                        |
|------------|----------------------------------------------|
| 13:42 UTC  | Rider trip bookings begin failing at >15%    |
| 13:44 UTC  | PagerDuty alert triggered for ETA service    |
| ...        | ...                                          |

## Root Cause(s)
- Primary:
- Contributing Factors:
- Missed Detection:

## What Went Well
- Auto-scaling mitigated surge
- Canary caught ETA model spike

## What Went Wrong
- Runbook outdated, required manual DB switch
- Kafka lag alert too noisy, ignored initially

## Action Items
| Owner       | Task                                      | SLA     |
|-------------|-------------------------------------------|---------|
| ETA Team    | Update feature rollout guardrail         | 2 weeks |
| SRE         | Recalibrate lag alert thresholds         | 1 week  |
| Security    | Review access logs for partner SDK leak  | 1 week  |

## Follow-ups
- Design Review Meeting Scheduled: [YYYY-MM-DD]
- Postmortem Retrospective Owner:

## Learnings & Prevention
- Propose new fitness function for model spike detection
- Add Terraform drift check to CI for this service

## Attachments
- Grafana Dashboards, PagerDuty Incident, Slack threads
```

---

## 4. Tooling and Storage
- **RCA GitHub Repo**: Markdown postmortems stored in `/postmortems/YYYY/MM/<incident>.md`
- **Templates**: Available in Backstage scaffolder and internal Docs
- **Slack Notifications**: Routed to #platform-postmortems via GitHub Action
- **Incident Tags**: All docs must tag affected services (e.g. `TripService`, `PricingModel`, `PartnerSDK`)

---

## 5. Roadmap
- **Q2**: Launch `rca-template.md` CLI + GitHub Action to auto-generate timeline tables
- **Q3**: Integrate RCA trends into monthly architecture reviews
- **Q4**: Machine-learning driven duplicate incident detection and RCA summarization

---

## Summary
The RideShareApp Platform postmortem system enables teams to learn, evolve, and improve architectural resilience collaboratively. Every incident is treated as an opportunity to reduce fragility and scale platform maturity.
