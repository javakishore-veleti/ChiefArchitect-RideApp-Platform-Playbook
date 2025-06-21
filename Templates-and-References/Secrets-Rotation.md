# Secrets Rotation Policy – RideShareApp Platform

## Objective
Define the lifecycle, automation, and governance for secrets management and rotation across the RideShareApp Platform to minimize security risks and maintain compliance.

---

## 1. Scope of Secrets Covered
- API keys (e.g., Maps, Payment gateways)
- OAuth tokens (GitHub, CI/CD tokens)
- Database credentials
- Kafka access secrets
- Model registry and ML pipeline tokens
- SSH keys and private keys
- Cloud provider IAM keys

---

## 2. Rotation Frequencies

| Secret Type              | Rotation Frequency | Notes |
|--------------------------|--------------------|-------|
| DB credentials (prod)    | Every 90 days      | Tied to ArgoCD rollout triggers |
| API keys (external)      | Every 180 days     | Exceptions require security approval |
| OAuth tokens             | Every 30 days      | Auto-refresh preferred |
| IAM keys (cloud)         | Every 90 days      | Use short-lived roles where possible |
| SSH keys                 | Quarterly          | Remove unused keys during rotation |

---

## 3. Rotation Methods
- Use AWS Secrets Manager or HashiCorp Vault for managed secrets
- Leverage CI/CD hooks to re-inject rotated secrets (e.g., GitHub Actions, Argo workflows)
- All rotations must:
  - Trigger a re-deploy of dependent service
  - Emit rotation event logs
  - Update access timestamp metadata

---

## 4. Automation Framework
- Vault policies use TTL + renewal grace periods
- GitHub Actions + Terraform track expiry metadata
- Slackbot alerts for secrets approaching rotation deadline (14 days in advance)
- Service teams maintain `secrets-config.yaml` with:
  - Owner
  - Last rotated date
  - Dependencies
  - TTL duration

---

## 5. Exceptions and Approvals
- Exceptions logged in security waiver registry
- Emergency extensions require:
  - Justification
  - Risk assessment
  - Security approval

---

## 6. Governance and Auditing
- Monthly audit of all production-bound secrets
- CI checks enforce secrets TTL metadata presence
- All secret rotations logged to SIEM and S3 audit log
- Audit failures surfaced in security OKRs

---

## Summary
This secrets rotation policy provides RideShareApp teams with a consistent, secure, and automated approach to managing secret lifecycles. Through time-bound controls, CI/CD integration, and observability, the platform ensures access integrity and operational resilience.
