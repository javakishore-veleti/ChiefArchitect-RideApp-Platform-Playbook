# Open Source Policy – RideShareApp Platform

## Objective
Define clear guidelines and governance for the use, contribution, and publication of open-source software (OSS) within the RideShareApp Platform to support transparency, innovation, and risk management.

---

## 1. Principles
- **Trust but Verify**: OSS must be vetted for security and licensing.
- **Community Stewardship**: Contribute meaningfully to upstream projects we rely on.
- **Strategic Contribution**: Open-source non-core tools that offer community value.
- **Compliance First**: Respect licenses, copyright, and attribution norms.

---

## 2. Approved Usage Guidelines
- Only use OSS libraries vetted by the Security or DevSecOps team.
- Preferred licenses: Apache 2.0, MIT, BSD. Avoid GPL unless dual-licensed.
- All third-party libraries must:
  - Be documented in `deps-oss.json` per service/module
  - Pass Snyk/OWASP scans before release
- External dependencies managed via package manager lockfiles (e.g., `pom.lock`, `package-lock.json`)

---

## 3. Contribution Policy
### a) Individual Developer Contributions
- Allowed under personal GitHub IDs with written approval
- Must disclose employment with RideShareApp in PR comments
- Code must not contain RideShareApp IP, PII, or platform logic

### b) Organization-Level Contributions
- Initiated via internal RFC with open-source steering committee
- Legal review of outbound license and contributor agreements
- OSS repo created under `github.com/ride-share-app-labs`
- README must declare governance, maintainers, and license

---

## 4. Releasing Internal Projects
- Qualify as:
  - Non-core infrastructure (e.g., observability tools, CLI scaffolds)
  - Platform extensions (e.g., partner SDKs, data loaders)
- Must include:
  - LICENSE, SECURITY.md, and CONTRIBUTING.md files
  - CI/CD pipelines with security scans
  - Dependency audit reports attached

---

## 5. Review & Approval Workflow
1. OSS Intake Form submission
2. Security Review (vulnerability + dependency audit)
3. Legal & License Check
4. Architecture Alignment Review (if part of platform interface)
5. Final Sign-off by OSS Steering Committee

---

## 6. Enforcement & Logging
- OSS usage auto-logged via SBOM generators
- Violations flagged during CI gates
- Monthly OSS hygiene reports sent to platform owners

---

## 7. Roadmap
- **Q2**: Launch OSS Request Tracker on internal Developer Portal
- **Q3**: Publish approved OSS usage registry (internal read-only)
- **Q4**: Host internal Open Source Summit with external contributors

---

## Summary
RideShareApp embraces the power of open-source while enforcing guardrails to ensure security, compliance, and ethical participation. This policy fosters both innovation and integrity in how we engage with the global developer ecosystem.
