# Executive Summary — ITGC Baseline Audit

| Field | Value |
|---|---|
| **Document ID** | ES-MT-2026-005 |
| **Version** | 1.0 |
| **Date** | May 16, 2026 |
| **Audience** | Board of Directors / Audit Committee |
| **Prepared By** | Michael Torres, CIA — Internal Audit Lead |

---

## Audit Opinion

The IT General Controls (ITGC) baseline assessment of **MediTrust Health** identified **14 findings** across the four domains tested: **2 Critical, 5 High, 5 Medium, and 2 Low**. The overall control posture is assessed as **PARTIALLY EFFECTIVE**. While foundational controls exist in user authentication, backup monitoring, and change logging, significant deficiencies in termination access removal, emergency change management, and segregation of duties analysis require immediate management attention.

This is MediTrust Health's **first formal ITGC audit**, and the results are consistent with expectations for an organization of MediTrust's size, growth trajectory, and audit maturity. The findings do not indicate a systemic control failure, but they do reflect scaling gaps in identity governance, change control discipline, and system monitoring coverage that are common in high-growth technology companies preparing for their initial SOC 2 engagement.

---

## Key Strengths Identified

- **Multi-Factor Authentication (MFA)**: MFA is enforced organization-wide across all systems, including those processing PHI. No exceptions were identified in sampled login events.
- **Backup Monitoring and Alerting**: Backup failure detection and alerting via Datadog is operating effectively. Three failures during the audit period were detected and resolved within SLA.
- **Change Logging Infrastructure**: Jira Service Management provides a comprehensive change management platform with tracking, classification, and audit trail capabilities.
- **Executive Commitment**: The executive team (CIO, CISO, VP Eng) has demonstrated strong support for the audit process and has proactively initiated remediation planning for critical and high-risk findings.

---

## Critical and High Findings Summary

### Critical (2)

| ID | Finding | Impact Summary |
|---|---|---|
| **UAM-F-03** | Delayed Termination Access Removal — 3 of 18 terminated employees had accounts active beyond the 24-hour SLA (one GitHub account persisted for 9 days) | Former employees retain access to PHI systems and source code — material data breach / HIPAA exposure risk |
| **CM-F-02** | Emergency Change Process Gaps — missing PIR on one emergency change; the same change was misclassified as emergency when it should have been standard | Non-urgent changes bypass standard controls; process integrity weakened |

### High (5)

| ID | Finding | Impact Summary |
|---|---|---|
| **UAM-F-04** | Privileged Access Key Rotation Not Enforced — two IAM keys exceeded 180 days | Extended exposure window if privileged keys are compromised |
| **CM-F-01** | Missing Business Justification in Change Requests — 2 of 30 changes undocumented | Changes cannot be risk-assessed; audit trail compromised |
| **CM-F-04** | SoD Weakness in Change Process — engineer self-approved and deployed to production | Single individual can introduce and deploy code without review |
| **BR-F-01** | Uncovered Production Resources — 2 EBS volumes not included in backup plans | PHI-derived data at risk of permanent loss |
| **SOD-F-02** | No Automated SoD Analysis — 12 users with incompatible role combinations identified | Users can perform conflicting actions without independent oversight |

---

## Risk Appetite Comparison

| Risk Domain | MediTrust Risk Appetite | Current Risk Exposure | Gap |
|---|---|---|---|
| **Access Management** | Low — no tolerance for unauthorized PHI access | **Moderate** — termination delays and provisioning gaps create exposure | Above appetite |
| **Change Management** | Low — changes must be authorized and tested | **Moderate** — emergency change misuse and staging bypasses weaken control | Above appetite |
| **Business Continuity** | Low — RTO of 4 hours for critical systems | **Low-to-Moderate** — most backups covered, but restoration test failed and uncovered resources exist | Slightly above appetite |
| **Segregation of Duties** | Low — incompatible duties must be identified | **Moderate** — no automated analysis, stale matrix, missing monitoring | Above appetite |

---

## Next Steps

1. **Immediate (0–30 days)**: Remediate both **Critical** findings — implement automated offboarding orchestration (UAM-F-03) and emergency change controls (CM-F-02).
2. **Short-term (30–90 days)**: Execute the **7 P2** (High- and Medium-risk) remediation items covering privileged access key rotation, change documentation, SoD analysis, branch protection, backup coverage, provisioning controls, and staging bypass prevention.
3. **Medium-term (90–180 days)**: Complete the remaining **5 P3** items including access recertification automation, restoration testing discipline, SoD monitoring, cross-region replication, and SoD matrix refresh.
4. **Continuous**: Establish a quarterly ITGC monitoring cadence to track control health between audit cycles. Begin SOC 2 Type II readiness activities in parallel with remediation.
5. **Risk Acceptance**: CIO to formally risk-accept any findings that cannot be remediated by the November 2026 deadline, with documented compensating controls and revised target dates. Audit Committee approval required for acceptances exceeding 90 days.

---

*This executive summary is based on audit work performed between January 1, 2026 and June 30, 2026. The findings and recommendations reflect the control environment as observed during that period. COBIT 2019 was used as the governing control framework. This audit was conducted in accordance with the International Standards for the Professional Practice of Internal Auditing (IIA Standards).*
