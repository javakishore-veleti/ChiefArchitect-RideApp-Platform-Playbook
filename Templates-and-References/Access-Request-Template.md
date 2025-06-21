# Access Request Template – RideShareApp Platform

Use this template to request temporary or permanent access to RideShareApp Platform systems. Submit as part of your pull request, Slackbot form, or internal access portal request.

---

## 1. Requestor Details
- **Name:**
- **Email:**
- **Team:**
- **Manager:**

## 2. Access Summary
- **Requested Role:** (e.g., `Platform Engineer`, `Pricing Analyst`)
- **Access Type:** (☐ Read ☐ Write ☐ Admin)
- **Resource(s) Requested:**
  - [ ] API: `pricing-service` (e.g., POST /surge)
  - [ ] Dashboard: `driver-compliance`
  - [ ] Infra: `staging cluster`
  - [ ] ML: `eta-model-registry`
  - [ ] Data: `trip_events.bigquery`

## 3. Justification
- **Business Purpose:**
  > e.g., Tuning surge config for New Year's Eve

- **Ticket or Epic Link:**
  > e.g., JIRA-1234 / GitHub Issue #99

## 4. Duration
- **Start Date:**
- **End Date:**
- **Is this Just-in-Time (JIT) access?** ☐ Yes ☐ No

## 5. Approvals
- **Domain Owner Approval:** (Name + Slack/email)
- **Security Team Approval:** (If admin or sensitive access)

## 6. Notes / Follow-up Tasks
> e.g., Schedule role revocation in Vault / SSO tool, notify SRE for secret rotation

---

## Submission Metadata (auto-filled if via form)
- Timestamp:
- Request ID:
- Status: Pending / Approved / Expired

---

## Policy Reference
See: [`RBAC-Policy.md`](./RBAC-Policy.md)
