# Healthcare Compliance as Code

Automated HIPAA control gap identification and remediation on a live patient intake API. This repo contains the infrastructure, policies, and remediation controls built to close 14 HIPAA Security Rule gaps before they became audit findings.

Forked from the CGE-P app starter -- a deliberately non-compliant patient intake API. The non-compliant starting state is intentional: it represents what most healthcare APIs look like before anyone has applied systematic compliance controls.

**Live interactive demo:** [chamb.dev/projects/hipaa-remediation.html](https://chamb.dev/projects/hipaa-remediation.html)

**Part of:** [Compliance Engineering Platform](https://chamb.dev/projects/compliance-platform.html)

---

## The problem this solves

A single unresolved HIPAA gap can trigger fines ranging from $100 to $50,000 per violation category. Most organizations discover these gaps during audits -- after the exposure has already occurred. This system finds and closes them before they become findings.

Healthcare APIs handle PHI across dozens of services. One misconfigured access policy, one public S3 bucket, one unencrypted resource -- any of these can trigger HIPAA breach notification requirements and the regulatory consequences that follow.

---

## What was built

**PHI detection and classification**
Amazon Macie scans S3 resources for PHI patterns -- SSNs, MRNs, dates of birth, clinical notes. Detection runs automatically. Findings write to Security Hub for centralized visibility.

**Preventive controls**
Service Control Policies block public exposure attempts before they occur. No manual review required. A public ACL on a PHI bucket is blocked at the API level, not caught after the fact.

**Automated remediation**
A Lambda function fires on every Macie finding. High-severity findings trigger immediate remediation -- public ACLs removed, encryption enforced, resource owner notified. The remediation action and timestamp are written to the evidence vault.

**Audit trail**
CloudTrail logs every access event. Security Hub aggregates findings across services. Every remediation action is cryptographically signed and stored in S3 Object Lock. Auditors get a complete, tamper-evident record.

**HIPAA control mapping**
Every control in this repo maps to a specific HIPAA Security Rule safeguard. Administrative, physical, and technical safeguards are all represented. The gap analysis document shows the before and after state for each of the 14 gaps closed.

---

## Business outcomes

- 14 HIPAA Security Rule control gaps identified and closed before audit
- PHI exposure attempts blocked at the API level before any resource goes public
- Audit preparation time reduced from manual evidence gathering to automated same-day collection
- Zero long-lived credentials in the remediation pipeline -- OIDC throughout

---

## Stack

| Service | Purpose |
|---------|---------|
| Amazon Macie | PHI detection and classification across S3 |
| AWS Lambda | Automated remediation on finding events |
| Service Control Policies | Preventive controls blocking public exposure |
| AWS Security Hub | Centralized finding aggregation and visibility |
| AWS CloudTrail | Complete audit trail of every access event |
| S3 Object Lock | Tamper-evident evidence vault |
| OIDC | Zero long-lived credentials in the pipeline |

---

## Why this is different

Most compliance automation is built by engineers who have never worked in a clinical environment. This was built by someone who has. Over a decade in radiology, MRI, CT, and emergency departments -- including COVID-era trauma bays -- means understanding not just what the regulation says but why the data being protected matters.

PHI is not an abstract compliance concept. It is a patient's cancer diagnosis, a psychiatric history, an HIV status. The controls in this repo exist to protect that.

---

## Related repo

[grc-engineering-capstone](https://github.com/chambsec/grc-engineering-capstone) -- the full Compliance Engineering Platform. This repo is one of five production systems built from that foundation.

---

## About

Built by Chris Chambers -- R.T.(R) ARRT, M.S. Cybersecurity. Over a decade of clinical healthcare experience across radiology, MRI, CT, and emergency departments, combined with cloud security engineering and compliance automation.

[chamb.dev](https://chamb.dev) | [LinkedIn](https://linkedin.com/in/chambsec) | [Resume](https://chamb.dev/resume.html)