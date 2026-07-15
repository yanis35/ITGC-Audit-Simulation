# Corrective Action Plan — ITGC Remediation

| Field | Value |
|---|---|
| **Document ID** | CAP-MT-2026-004 |
| **Version** | 1.0 |
| **Date** | May 19, 2026 |
| **Prepared By** | Michael Torres, CIA — Internal Audit Lead |
| **Approved By** | Dr. Elena Vasquez — Chief Information Officer |
| **Remediation Horizon** | May 19, 2026 – November 20, 2026 (6 months) |

---

## Remediation Summary

| Priority | Count | Definition |
|---|---|---|
| **P1 — Immediate (0–30 days)** | 2 | Critical-risk findings requiring immediate remediation |
| **P2 — Short-term (30–90 days)** | 7 | High-risk findings requiring remediation within the quarter |
| **P3 — Medium-term (90–180 days)** | 5 | Medium- and Low-risk findings requiring remediation before audit close |
| **Total** | **14** | — |

## Tracking Metrics

| Metric | Target | Current |
|---|---|---|
| Findings Remediated | 100% by Nov 20, 2026 | 0 / 14 (0%) |
| P1 Findings Remediated | 100% by Jun 19, 2026 | 0 / 2 (0%) |
| P2 Findings Remediated | 100% by Aug 19, 2026 | 0 / 7 (0%) |
| P3 Findings Remediated | 100% by Nov 20, 2026 | 0 / 5 (0%) |
| Overdue Remediation Items | 0 | — |
| Recurring Findings (from future audits) | 0 | N/A (baseline audit) |

---

## Remediation Plan

| ID | Domain | Finding Title | Risk Rating | Priority | Action Required | Owner | Due Date | Status |
|---|---|---|---|---|---|---|---|---|
| UAM-F-03 | User Access Management | Delayed Termination Access Removal | **Critical** | **P1** | Implement automated offboarding orchestration via Okta Lifecycle Management; deploy daily reconciliation report of active employees vs. system accounts; configure real-time alerting for deactivations exceeding 4 hours. | James Okonkwo, CISO | Jun 19, 2026 | **In Progress** |
| CM-F-02 | Change Management | Emergency Change Process Gaps | **Critical** | **P1** | Implement pre-submission emergency classification validation in Jira; establish monthly retrospective review of emergency changes; deliver refresher training on emergency change criteria. | Sarah Chen, VP Eng | Jun 19, 2026 | **In Progress** |
| UAM-F-04 | User Access Management | Privileged Access Key Rotation Not Enforced | **High** | **P2** | Configure AWS IAM access key rotation policy (90-day max); update privileged access review checklist to include key-age verification; implement automated key expiry notifications. | James Okonkwo, CISO | Jul 19, 2026 | **Planned** |
| CM-F-01 | Change Management | Missing Business Justification in Change Requests | **High** | **P2** | Enforce mandatory business justification and risk classification fields in Jira for all change types; deliver SRE team training on change policy; conduct retrospective review of undocumented changes. | Sarah Chen, VP Eng | Jul 19, 2026 | **Planned** |
| CM-F-04 | Change Management | Segregation of Duties Weakness in Change Process | **High** | **P2** | Audit and remediate GitHub branch protection rules across all production repositories; implement quarterly branch protection compliance review; enable Code Owners review requirement. | Sarah Chen, VP Eng | Jul 19, 2026 | **Planned** |
| BR-F-01 | Backup and Recovery | Production Resources Not Included in Backup Plans | **High** | **P2** | Add uncovered EBS volumes to AWS Backup plan; implement AWS Config rule for backup plan coverage detection; update IaC templates to include backup plan associations. | Ravi Patel, Data Engineer (SRE) | Aug 19, 2026 | **Planned** |
| SOD-F-02 | Segregation of Duties | No Automated SoD Analysis Performed | **High** | **P2** | Procure or build automated SoD analysis capability; remediate 12 identified incompatible combinations within 30 days of tool deployment; require documented compensating controls for unavoidable conflicts. | James Okonkwo, CISO | Aug 19, 2026 | **Planned** |
| UAM-F-01 | User Access Management | User Provisioning Entitlement Exceptions | **Medium** | **P2** | Implement role-based access templates in Okta and AWS IAM Identity Center; configure Jira to require role template selection; deploy post-provisioning entitlement validation check. | James Okonkwo, CISO | Aug 19, 2026 | **Planned** |
| CM-F-03 | Change Management | Staging Environment Bypass Without Documented Exception | **Medium** | **P2** | Integrate exception approval gate into CI/CD pipeline; implement weekly report of direct-to-production deployments for management review. | Sarah Chen, VP Eng | Aug 19, 2026 | **Planned** |
| UAM-F-02 | User Access Management | Access Recertification Campaign Incompletion | **Medium** | **P3** | Automate HRIS-to-Okta reconciliation prior to each recertification campaign; implement pre-campaign compliance check to flag inactive or terminated users. | James Okonkwo, CISO | Oct 19, 2026 | **Planned** |
| BR-F-02 | Backup and Recovery | Restoration Test Failure Without Root Cause Analysis | **Medium** | **P3** | Perform RCA for Q2 restoration test failure; implement policy requiring RCA for all failed restoration tests within 5 business days; align DR account configuration management with production standards. | Ravi Patel, Data Engineer (SRE) | Oct 19, 2026 | **Planned** |
| SOD-F-03 | Segregation of Duties | Insufficient SoD-Specific Monitoring in SIEM | **Medium** | **P3** | Develop and deploy SoD-specific detection rules in Datadog; include SoD monitoring scenarios in security playbook; conduct quarterly rule-effectiveness review. | James Okonkwo, CISO | Oct 19, 2026 | **Planned** |
| BR-F-03 | Backup and Recovery | Cross-Region Replication Gaps for DynamoDB Backups | **Low** | **P3** | Configure AWS Backup cross-region copy for DynamoDB tables; implement separate KMS key for secondary region; document risk acceptance if deferred. | Ravi Patel, Data Engineer (SRE) | Nov 20, 2026 | **Planned** |
| SOD-F-01 | Segregation of Duties | SoD Matrix Outdated | **Low** | **P3** | Assign IAM team ownership for SoD matrix maintenance; establish semi-annual review cycle; expand matrix to cover Datadog, PagerDuty, and other new platforms. | James Okonkwo, CISO | Nov 20, 2026 | **Planned** |

---

## Governance and Reporting

- **Weekly Status Reviews**: Every Friday, the remediation owners will provide a status update to the Audit Steering Committee (CIO, CISO, VP Eng, Internal Audit Lead).
- **Monthly CAP Dashboard**: A consolidated dashboard will be presented to the Audit Committee showing remediation progress against targets, overdue items, and risk acceptance requests.
- **Closure Verification**: Each finding will be closed only after the Internal Audit Lead has reviewed and accepted the remediation evidence, including re-performance of the control where applicable.
- **Risk Acceptance**: Any finding not remediated by the due date must be formally risk-accepted by the CIO with a documented rationale and revised target date. Risk acceptances exceeding 90 days require Audit Committee approval.
