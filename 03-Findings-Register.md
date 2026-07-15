# Findings Register — Risk-Rated ITGC Deficiencies

| Field | Value |
|---|---|
| **Document ID** | FR-MT-2026-003 |
| **Version** | 1.0 |
| **Date** | May 16, 2026 |
| **Prepared By** | Priya Nair, CISA — IT Audit Senior |
| **Reviewed By** | Michael Torres, CIA — Internal Audit Lead |

---

## Summary by Severity

| Severity | Count | Percentage |
|---|---|---|
| **Critical** | 2 | 14.3% |
| **High** | 5 | 35.7% |
| **Medium** | 5 | 35.7% |
| **Low** | 2 | 14.3% |
| **Total** | **14** | **100%** |

### Breakdown by Domain

| Domain | Total | Critical | High | Medium | Low |
|---|---|---|---|---|---|
| User Access Management | 4 | 1 | 1 | 2 | 0 |
| Change Management | 4 | 1 | 2 | 1 | 0 |
| Backup and Recovery | 3 | 0 | 1 | 1 | 1 |
| Segregation of Duties | 3 | 0 | 1 | 1 | 1 |
| **Total** | **14** | **2** | **5** | **5** | **2** |

---

## Findings Detail

### Domain 1: User Access Management

---

#### UAM-F-01 — User Provisioning Entitlement Exceptions

| Attribute | Detail |
|---|---|
| **Domain** | User Access Management |
| **Finding Title** | User Provisioning Entitlement Exceptions |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | DSS05.04 — Manage access identities and enable access |
| **Description** | Two of 25 sampled users (8%) were provisioned with database read–write access that exceeded their approved role profile. Both instances were self-detected and corrected within 48 hours, but the provisioning process lacks automated entitlement validation against approved role templates. |
| **Root Cause** | Provisioning workflow relies on manual entitlement selection by the IT support team without automated guardrails or role-based templates enforced at the point of request fulfillment. |
| **Impact** | Users may receive excessive database permissions, increasing the risk of unauthorized PHI access, data exfiltration, or accidental modification of production data. |
| **Recommendation** | Implement role-based access templates in Okta and AWS IAM Identity Center; configure Jira Service Management to require role template selection rather than free-form entitlement entry; deploy a post-provisioning entitlement validation check within 24 hours. |

---

#### UAM-F-02 — Access Recertification Incompletion

| Attribute | Detail |
|---|---|
| **Domain** | User Access Management |
| **Finding Title** | Access Recertification Campaign Incompletion |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | APO13.01 — Establish and maintain an ISMS |
| **Description** | The Q1 2026 access recertification campaign achieved only 97% completion — nine user accounts were not certified within the 14-day campaign window. These accounts belonged to a terminated contractor group that was still present in the campaign population, indicating a reconciliation failure between HR termination records and the access review scope. |
| **Root Cause** | The recertification campaign population is derived from a static user list that is not automatically reconciled with HR termination data prior to campaign launch. |
| **Impact** | User accounts may retain access privileges beyond the point at which they are warranted, increasing the risk of orphaned accounts and unauthorized access to PHI systems. |
| **Recommendation** | Automate reconciliation between HRIS termination records and the Okta campaign population prior to each recertification cycle; implement a pre-campaign compliance check that flags and removes terminated or inactive users before the campaign is launched. |

---

#### UAM-F-03 — Delayed Termination Access Removal

| Attribute | Detail |
|---|---|
| **Domain** | User Access Management |
| **Finding Title** | Delayed Termination Access Removal |
| **Risk Rating** | **Critical** |
| **COBIT Reference** | APO07.06 — Manage employee changes |
| **Description** | Three of 18 terminated employees (16.7%) experienced account deactivation delays exceeding the 24-hour SLA. One GitHub account remained active for 9 days post-termination, one Okta group membership was not removed for 4 days, and one Slack account remained active for 36 hours. No formal escalation or alert was triggered for any of the delayed deactivations. |
| **Root Cause** | Termination offboarding procedures are executed manually across multiple systems (Okta, AWS, GitHub, Slack, Jira) with no centralized orchestration or automated cross-system deactivation workflow. No monitoring alert exists for missed or delayed deactivations. |
| **Impact** | Former employees retain access to PHI-processing systems, source code repositories, and internal communication platforms, creating a material risk of data breach, IP theft, or HIPAA violation. |
| **Recommendation** | Implement an automated offboarding orchestration workflow via Okta Lifecycle Management or a SCIM-based integration to deprovision accounts across all systems upon HRIS termination trigger; deploy a daily reconciliation report comparing active employees against active system accounts; set up real-time alerting for any deactivation exceeding 4 hours. |

---

#### UAM-F-04 — Privileged Access Key Rotation

| Attribute | Detail |
|---|---|
| **Domain** | User Access Management |
| **Finding Title** | Privileged Access Key Rotation Not Enforced |
| **Risk Rating** | **High** |
| **COBIT Reference** | DSS06.03 — Manage roles, responsibilities, and access privileges |
| **Description** | Two of 34 privileged IAM users had active access keys exceeding 180 days. The organization does not enforce automated key rotation for IAM users with administrative privileges. While quarterly privileged access reviews are conducted, the review does not specifically verify key age or rotation compliance. |
| **Root Cause** | No automated access key rotation policy is configured in AWS IAM; privileged access review checklists do not include a key-age verification step. |
| **Impact** | Long-lived access keys increase the window of exposure if a key is compromised. An attacker with a leaked privileged access key could gain persistent administrative access to AWS environments hosting PHI. |
| **Recommendation** | Configure AWS IAM access key rotation policy (maximum 90 days) using IAM credential reports and AWS Config rules; update the privileged access review checklist to include key age verification; implement automated key expiry notifications. |

---

### Domain 2: Change Management

---

#### CM-F-01 — Missing Change Documentation

| Attribute | Detail |
|---|---|
| **Domain** | Change Management |
| **Finding Title** | Missing Business Justification in Change Requests |
| **Risk Rating** | **High** |
| **COBIT Reference** | BAI06.01 — Manage changes — change standards and procedures |
| **Description** | Two of 30 sampled standard changes (6.7%) lacked documented business justification. Both were infrastructure scaling changes executed by the SRE team and logged under a generic "operations" category with no explanation of the business need, risk assessment, or expected outcome. |
| **Root Cause** | Jira Service Management change request form does not enforce mandatory fields for business justification on changes classified as "infrastructure" or "operations." SRE team members were not aware that business justification is required for all change types. |
| **Impact** | Undocumented changes cannot be adequately risk-assessed or reviewed, increasing the likelihood of unauthorized or poorly considered changes affecting PHI system availability or integrity. Regulators and auditors may view missing documentation as a control breakdown. |
| **Recommendation** | Enforce mandatory business justification and risk classification fields for all change request types in Jira; provide training to the SRE team on change management policy; conduct a one-time review of all changes lacking justification to assess impact. |

---

#### CM-F-02 — Emergency Change Process Gaps

| Attribute | Detail |
|---|---|
| **Domain** | Change Management |
| **Finding Title** | Emergency Change Process Gaps |
| **Risk Rating** | **Critical** |
| **COBIT Reference** | BAI06.03 — Manage emergency changes |
| **Description** | One of seven emergency changes (JIRA-3055) had no documented post-implementation review (PIR). Further analysis revealed the change — a hotfix for a non-critical data display issue — did not meet the organization's emergency change criteria and should have been classified as a standard change. This represents both a process compliance failure and a potential bypass of standard change controls. |
| **Root Cause** | The emergency change classification criteria are not enforced within Jira; any user can select "Emergency" without validation against defined criteria. No periodic review of emergency change appropriateness is performed. |
| **Impact** | Non-urgent changes that bypass standard testing and approval gates increase the risk of production incidents, data corruption, and undetected configuration errors. Classification abuse erodes the integrity of the change management process. |
| **Recommendation** | Implement a pre-submission validation gate in Jira that requires emergency change justification against defined criteria; perform a monthly retrospective review of all emergency changes for classification accuracy; provide refresher training on emergency change criteria to all change initiators. |

---

#### CM-F-03 — Staging Environment Bypass

| Attribute | Detail |
|---|---|
| **Domain** | Change Management |
| **Finding Title** | Staging Environment Bypass Without Documented Exception |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | BAI03.01 — Manage the identification and build of solutions |
| **Description** | Two of 25 sampled production deployments bypassed the staging environment — one direct database hotpatch and one configuration-only change via Ansible. Both had operational justifications but lacked documented exception approval as required by policy. |
| **Root Cause** | The CI/CD pipeline allows direct-to-production deployments for certain change types without requiring an approved exception ticket. The exception approval workflow is not integrated into the deployment pipeline, making it possible to bypass staging without triggering a policy violation. |
| **Impact** | Untested changes deployed directly to production increase the risk of service degradation, data integrity issues, and undetected defects. The absence of a documented exception erodes audit trail integrity. |
| **Recommendation** | Integrate an exception approval gate into the CI/CD pipeline (ArgoCD / GitHub Actions) that requires a valid, approved Jira exception ticket before bypassing staging is allowed; implement a weekly report of all direct-to-production deployments for management review. |

---

#### CM-F-04 — Segregation of Duties Weakness in Change Process

| Attribute | Detail |
|---|---|
| **Domain** | Change Management |
| **Finding Title** | Segregation of Duties Weakness in Change Process |
| **Risk Rating** | **High** |
| **COBIT Reference** | BAI06.01 / DSS06.02 — Change management and segregation of duties |
| **Description** | In one of 25 sampled deployments, the same engineer authored the code, self-approved the pull request, and deployed to production. GitHub branch protection rules for the infrastructure repository did not enforce a required reviewer policy, allowing self-approval. This violates the segregation of duties principle. |
| **Root Cause** | Branch protection rules in the infrastructure repository were misconfigured — the "Require pull request reviews before merging" setting was enabled but the "Dismiss stale pull request approvals" and "Require review from Code owners" settings were not. An oversight during repository setup allowed the engineer to self-approve. |
| **Impact** | A single individual can introduce and deploy code changes without independent review, increasing the risk of malicious code injection, misconfiguration, or undetected errors affecting PHI systems. |
| **Recommendation** | Audit and remediate branch protection rules across all production repositories to enforce mandatory code review by at least one reviewer who is not the author; implement a quarterly review of branch protection rule compliance; enable GitHub's "Require review from Code Owners" for all production branches. |

---

### Domain 3: Backup and Recovery

---

#### BR-F-01 — Uncovered Production Resources

| Attribute | Detail |
|---|---|
| **Domain** | Backup and Recovery |
| **Finding Title** | Production Resources Not Included in Backup Plans |
| **Risk Rating** | **High** |
| **COBIT Reference** | DSS04.01 — Define backup and recovery requirements |
| **Description** | Two of 44 in-scope resources — unencrypted EBS volumes in us-west-2 used by a legacy analytics pipeline — were not included in any automated backup plan. These volumes contained de-identified PHI-derived datasets (not direct PHI, but derived from it). The resources were discovered during the audit via a cross-reference of the AWS resource inventory against the backup plan membership. |
| **Root Cause** | The legacy analytics pipeline was deployed before the current backup policy was established. New resource onboarding procedures do not include a mandatory step to add resources to backup plans. The infrastructure-as-code templates for this pipeline did not include backup plan association. |
| **Impact** | De-identified PHI-derived data is at risk of permanent loss in the event of an EBS volume failure or regional outage. While derived data is lower risk than direct PHI, loss of this data would impact business operations and reporting. |
| **Recommendation** | Add the two uncovered EBS volumes to the AWS Backup plan immediately; implement an automated AWS Config rule to detect resources without backup plan association; update infrastructure-as-code templates to include backup plan associations for all EBS volumes. |

---

#### BR-F-02 — Restoration Test Failure With No Root Cause Analysis

| Attribute | Detail |
|---|---|
| **Domain** | Backup and Recovery |
| **Finding Title** | Restoration Test Failure Without Root Cause Analysis |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | DSS04.03 — Test backup and recovery |
| **Description** | The Q2 2026 restoration test failed due to a misconfigured IAM role in the disaster recovery account. The restoration was eventually completed using an alternate method after 7 hours (exceeding the 4-hour RTO). The failure was documented in an incident ticket, but no root cause analysis (RCA) was performed to prevent recurrence. |
| **Root Cause** | The IAM role assumed during restoration testing had a stale trust policy that did not include the DR account's current AWS Organization ID. The DR environment is not subject to the same configuration management controls as the production environment. There is no policy requiring RCA for failed restoration tests. |
| **Impact** | A restoration test failure without RCA means the underlying configuration issue remained unresolved, potentially affecting a real recovery scenario. The 7-hour recovery time exceeded RTO by 75%, which could result in extended downtime and SLA penalties during an actual disaster. |
| **Recommendation** | Perform a root cause analysis for the Q2 restoration test failure and remediate the IAM role configuration; implement a policy requiring RCA for all failed restoration tests within 5 business days; align DR account configuration management with production standards. |

---

#### BR-F-03 — Cross-Region Replication Gaps

| Attribute | Detail |
|---|---|
| **Domain** | Backup and Recovery |
| **Finding Title** | Cross-Region Replication Gaps for DynamoDB Backups |
| **Risk Rating** | **Low** |
| **COBIT Reference** | DSS04.01 — Define backup and recovery requirements |
| **Description** | DynamoDB point-in-time recovery (PITR) backups are not replicated across AWS regions. Additionally, the secondary region backup vault uses the same KMS key as the primary region, creating a single-point-of-failure risk — if the primary region KMS key is compromised, the secondary region backups become inaccessible. |
| **Root Cause** | The backup architecture design did not account for DynamoDB PITR cross-region replication requirements. The KMS key strategy was designed for simplicity rather than resilience. |
| **Impact** | In a regional disaster affecting us-east-1, DynamoDB data could only be recovered from PITR in the primary region. If the KMS key is compromised in the primary region, secondary region backups would be unrecoverable. |
| **Recommendation** | Configure AWS Backup cross-region copy for DynamoDB tables; implement a separate KMS key for the secondary region with a cross-region key policy; document the risk acceptance if remediation is deferred. |

---

### Domain 4: Segregation of Duties

---

#### SOD-F-01 — Stale Segregation of Duties Matrix

| Attribute | Detail |
|---|---|
| **Domain** | Segregation of Duties |
| **Finding Title** | SoD Matrix Outdated |
| **Risk Rating** | **Low** |
| **COBIT Reference** | DSS06.02 — Segregation of duties |
| **Description** | The SoD matrix has not been updated since November 2024 (18 months stale). It does not cover recently adopted platforms (Datadog, PagerDuty). Two incompatible combinations identified during the audit (GitHub admin + Jira admin, AWS network admin + security group manager) are not documented in the matrix. |
| **Root Cause** | No owner or recurring schedule is assigned for SoD matrix maintenance. The matrix was created as a one-time exercise and was not refreshed when new systems were onboarded. |
| **Impact** | An incomplete SoD matrix means incompatible role combinations can go undetected until a violation occurs. Users may hold roles in new platforms that conflict with existing platform roles without any compensating control review. |
| **Recommendation** | Assign ownership for SoD matrix maintenance to the IAM team; establish a semi-annual review cycle; expand the matrix to cover Datadog, PagerDuty, and any other systems onboarded since the last update. |

---

#### SOD-F-02 — Missing Automated Segregation of Duties Analysis

| Attribute | Detail |
|---|---|
| **Domain** | Segregation of Duties |
| **Finding Title** | No Automated SoD Analysis Performed |
| **Risk Rating** | **High** |
| **COBIT Reference** | APO07.05 — Define and communicate roles and responsibilities |
| **Description** | No formal automated SoD analysis was performed during the audit period. The audit team's re-performance of an entitlement cross-match identified 12 users with potentially incompatible role combinations — including developers with production database write access and infrastructure engineers holding both GitHub admin and AWS admin roles. None of the affected users had documented compensating controls. |
| **Root Cause** | MediTrust Health does not have a tool or process for automated cross-system entitlement analysis. Role configurations are managed in individual system silos (AWS, GitHub, Okta) with no central identity governance platform capable of SoD analysis. |
| **Impact** | Users with incompatible roles can perform conflicting actions (e.g., develop, test, and promote code to production) without independent oversight, increasing the risk of fraud, data manipulation, and undetected errors in PHI systems. |
| **Recommendation** | Procure or build an automated SoD analysis capability — either via a cloud identity governance tool (e.g., SailPoint, Okta Identity Governance) or via a scheduled custom script that cross-references entitlement exports from all in-scope systems; remediate the 12 identified incompatible combinations within 30 days; require documented compensating controls for any unavoidable SoD conflicts. |

---

#### SOD-F-03 — Insufficient SoD-Specific Monitoring Rules

| Attribute | Detail |
|---|---|
| **Domain** | Segregation of Duties |
| **Finding Title** | Insufficient SoD-Specific Monitoring in SIEM |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | MEA03.01 — Monitor compliance with internal policies |
| **Description** | Datadog Security Monitoring is configured for access violation detection but does not include rules specific to SoD violations — such as a single user performing privileged actions across multiple systems (e.g., creating an IAM role in AWS and then approving a GitHub PR referencing that role). All 15 sampled alerts were infrastructure-focused. |
| **Root Cause** | The SIEM implementation prioritized infrastructure-level threat detection over identity-level SoD monitoring. The security team's monitoring playbook does not include SoD-specific use cases. |
| **Impact** | SoD violations that manifest as cross-system administrative actions may go undetected by automated monitoring, requiring manual investigation to identify. This increases dwell time for potential insider threats or privilege abuse scenarios. |
| **Recommendation** | Develop and deploy SoD-specific detection rules in Datadog (e.g., alerts for any user who performs an action in two or more systems within a defined time window where those systems represent incompatible combinations); include SoD monitoring scenarios in the security monitoring playbook; conduct a quarterly rule-effectiveness review. |
