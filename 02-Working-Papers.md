# Working Papers — ITGC Test Scripts and Evidence

| Field | Value |
|---|---|
| **Document ID** | WP-MT-2026-002 |
| **Version** | 1.0 |
| **Audit Period** | January 1 – June 30, 2026 |
| **Prepared By** | Priya Nair, CISA — IT Audit Senior |
| **Reviewed By** | Michael Torres, CIA — Internal Audit Lead |

---

## Domain 1: User Access Management (UAM)

### UAM-01 — User Provisioning Process

| Attribute | Detail |
|---|---|
| **Procedure ID** | **UAM-01** |
| **Control Objective** | New user accounts are provisioned only after authorized request and manager approval, with access rights consistent with job responsibilities. |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | DSS05.04 — Manage access identities and enable access |
| **Test Steps** | 1. Select a sample of 25 users provisioned during the audit period from the HR onboarding log. 2. Trace each user to the corresponding access request ticket in Jira Service Management. 3. Verify manager approval was recorded. 4. Compare provisioned entitlements against the approved access profile. 5. Verify account creation timestamp falls after approval timestamp. |
| **Expected Result** | 100% of sampled users have documented manager approval; entitlements match the approved profile; no accounts created prior to approval. |
| **Actual Result** | 23 of 25 samples met all criteria. Two users (IDs 4421, 4489) were provisioned with elevated database read–write access that exceeded their approved role profile. Both instances were corrected within 48 hours. |
| **Evidence Reference** | Jira tickets: ITSEC-3421, ITSEC-3422, ITSEC-3510... HR onboarding log Q1–Q2 2026 (HR-ON-2026.xlsx) |
| **Deficiency** | Yes — see finding **UAM-F-01** |

---

### UAM-02 — Access Recertification

| Attribute | Detail |
|---|---|
| **Procedure ID** | **UAM-02** |
| **Control Objective** | User access rights are recertified by managers on a quarterly basis to ensure continued appropriateness. |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | APO13.01 — Establish and maintain an ISMS |
| **Test Steps** | 1. Obtain the Q1 and Q2 2026 access recertification campaign reports from Okta. 2. Verify campaign scope included all active users. 3. Check that managers completed recertification within the defined campaign window. 4. Select a sample of 20 users and confirm recertification sign-off. 5. Review any access changes resulting from recertification. |
| **Expected Result** | All active users included in the campaign; 100% manager completion rate within the designated 14-day window. |
| **Actual Result** | Q1 campaign had 97% completion (9 users not certified). Q2 campaign had 100% completion. The nine Q1 outliers were carryover accounts from a terminated contractor group that was not removed from the campaign population. |
| **Evidence Reference** | Okta Advanced Server Access — Campaign Reports Q1-2026, Q2-2026; Termination log Jan–Mar 2026 |
| **Deficiency** | Yes — see finding **UAM-F-02** |

---

### UAM-03 — Termination Access Removal

| Attribute | Detail |
|---|---|
| **Procedure ID** | **UAM-03** |
| **Control Objective** | User access is revoked within 24 hours of employee termination or role change. |
| **Risk Rating** | **Critical** |
| **COBIT Reference** | APO07.06 — Manage employee changes |
| **Test Steps** | 1. Extract the list of 18 employee terminations during the audit period from the HRIS. 2. For each terminated employee, check Okta SSO deactivation timestamp. 3. Verify AWS IAM user/key deletion or deactivation. 4. Verify GitHub Enterprise account suspension. 5. Check Jira and Slack deprovisioning. 6. Measure elapsed time between HR termination effective date and last access deactivation. |
| **Expected Result** | All accounts deactivated within 24 hours of the HR termination effective date. |
| **Actual Result** | Three of 18 terminated employees had account deactivation delays exceeding 24 hours: one by 9 days (GitHub account remained active), one by 4 days (Okta group membership not removed), and one by 36 hours (Slack account). |
| **Evidence Reference** | HR termination report Q1-Q2-2026 (HR-TERM-2026.xlsx); Okta deactivation audit log; AWS IAM credential report (weekly); GitHub audit log |
| **Deficiency** | Yes — see finding **UAM-F-03** |

---

### UAM-04 — Privileged Access Management

| Attribute | Detail |
|---|---|
| **Procedure ID** | **UAM-04** |
| **Control Objective** | Privileged (administrative) accounts are uniquely assigned, require MFA, and are monitored for anomalous usage. |
| **Risk Rating** | **High** |
| **COBIT Reference** | DSS06.03 — Manage roles, responsibilities, and access privileges |
| **Test Steps** | 1. Obtain the list of all users with administrative privileges in AWS (console and CLI), GitHub, and Okta. 2. Verify each privileged account is tied to a named individual (no shared or generic admin accounts). 3. Confirm MFA is enforced for all privileged accounts. 4. Review a sample of 15 privileged session logs from Datadog for anomalous activity. 5. Verify privileged access review occurs quarterly. |
| **Expected Result** | 100% named accounts with MFA enforced; quarterly reviews completed; no anomalies in sampled sessions. |
| **Actual Result** | All 34 privileged accounts are named and MFA-enforced. Q1 privileged access review was completed on time. However, two AWS IAM users had active access keys exceeding 180 days (key rotation not enforced). No anomalous activity was identified in the sampled sessions. |
| **Evidence Reference** | AWS IAM credential report (snapshot Apr 2026); Okta MFA enforcement policy; Privileged access review Q1-2026; Datadog session logs (15 samples) |
| **Deficiency** | Yes — see finding **UAM-F-04** |

---

### UAM-05 — Authentication Controls (MFA & SSO)

| Attribute | Detail |
|---|---|
| **Procedure ID** | **UAM-05** |
| **Control Objective** | Multi-factor authentication is enforced for all user access to systems processing PHI, and SSO is the primary authentication mechanism. |
| **Risk Rating** | **Low** |
| **COBIT Reference** | DSS05.05 — Manage logical access |
| **Test Steps** | 1. Review Okta MFA policy configuration for all applications. 2. Verify that PHI-processing systems (Core Platform, AWS RDS, Datadog logs) require MFA. 3. Sample 30 user login events from Okta and confirm MFA challenge was enforced. 4. Identify any non-SSO authentication paths. |
| **Expected Result** | MFA enforced for all PHI-adjacent systems; 100% of sampled logins required MFA challenge; no local authentication bypasses identified. |
| **Actual Result** | MFA is enforced organization-wide. All 30 sampled logins included MFA challenge. One legacy API gateway endpoint allowed token-based authentication without MFA for internal service accounts — this is documented and accepted. |
| **Evidence Reference** | Okta MFA policy report; Okta system log (30 sampled events); API gateway configuration |
| **Deficiency** | No — observation only |

---

## Domain 2: Change Management (CM)

### CM-01 — Change Request Process

| Attribute | Detail |
|---|---|
| **Procedure ID** | **CM-01** |
| **Control Objective** | All changes to the production environment are initiated through a standardized change request process with documented business justification. |
| **Risk Rating** | **High** |
| **COBIT Reference** | BAI06.01 — Manage changes — change standards and procedures |
| **Test Steps** | 1. Obtain the change log from Jira Service Management for the audit period (all production changes). 2. Select a sample of 30 standard changes. 3. Verify each change has a unique ticket ID, description, business justification, and risk classification. 4. Confirm classification (Standard / Normal / Emergency) is documented. 5. Verify all required fields are populated. |
| **Expected Result** | 100% of sampled changes have complete documentation including business justification and risk classification. |
| **Actual Result** | 28 of 30 samples met all criteria. Two changes (JIRA-2891, JIRA-3012) lacked documented business justification — both were infrastructure scaling changes executed by the SRE team under "operations" with no formal justification recorded. |
| **Evidence Reference** | Jira Service Management change log Q1-Q2-2026; sample ticket exports |
| **Deficiency** | Yes — see finding **CM-F-01** |

---

### CM-02 — Emergency Change Process

| Attribute | Detail |
|---|---|
| **Procedure ID** | **CM-02** |
| **Control Objective** | Emergency changes are subject to a defined expedited process that includes post-implementation review and retrospective approval. |
| **Risk Rating** | **Critical** |
| **COBIT Reference** | BAI06.03 — Manage emergency changes |
| **Test Steps** | 1. Identify all emergency changes in the audit period from the change log. 2. Verify each emergency change followed the expedited workflow. 3. Confirm retrospective approval was obtained within 5 business days. 4. Verify post-implementation review (PIR) was completed. 5. Assess whether changes classified as emergency genuinely met the emergency criteria. |
| **Expected Result** | All emergency changes have retrospective approval, completed PIR, and valid emergency justification. |
| **Actual Result** | Seven emergency changes were identified. Six of seven had retrospective approval and PIR within the required window. One emergency change (JIRA-3055, a hotfix to address a data display issue) lacked documented PIR and was retrospectively classified as emergency — review of the issue indicated it should have been classified as a standard priority change. |
| **Evidence Reference** | Jira Service Management emergency change log; PIR records; incident post-mortems |
| **Deficiency** | Yes — see finding **CM-F-02** |

---

### CM-03 — Testing and Approval Before Production Deployment

| Attribute | Detail |
|---|---|
| **Procedure ID** | **CM-03** |
| **Control Objective** | Changes are tested in a non-production environment and approved prior to deployment to production. |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | BAI03.01 — Manage the identification and build of solutions |
| **Test Steps** | 1. Select a sample of 25 production deployments from the CI/CD pipeline (GitHub Actions + ArgoCD). 2. Verify each deployment was preceded by a successful build and test stage in a staging environment. 3. Confirm code review approval was obtained via pull request. 4. Verify production deployment required a separate approval gate. |
| **Expected Result** | 100% of deployments preceded by staging tests and code review approval; production deployment gated. |
| **Actual Result** | 23 of 25 deployments met all criteria. Two deployments bypassed the staging environment: one was a direct database hotpatch (emergency), and one was a configuration-only change pushed directly to production via Ansible. Both exceptions had operational justifications but lacked documented exception approval. |
| **Evidence Reference** | GitHub Actions run logs; ArgoCD deployment history; pull request approvals (25 samples) |
| **Deficiency** | Yes — see finding **CM-F-03** |

---

### CM-04 — Segregation of Duties in Change Process

| Attribute | Detail |
|---|---|
| **Procedure ID** | **CM-04** |
| **Control Objective** | Changes are developed, tested, and promoted to production by separate individuals to maintain segregation of duties. |
| **Risk Rating** | **High** |
| **COBIT Reference** | BAI06.01 / DSS06.02 — Change management and segregation of duties |
| **Test Steps** | 1. For the sample of 25 deployments used in CM-03, identify the individuals who authored the code, approved the PR, and deployed to production. 2. Verify that no single individual performed two or more of these roles for the same change. 3. Check GitHub branch protection rules requiring at least one reviewer who is not the author. 4. For emergency changes, verify compensating controls were documented. |
| **Expected Result** | No single individual both authored and deployed a change without independent review; branch protection rules enforce separation. |
| **Actual Result** | 24 of 25 samples had proper separation. In one case (JIRA-3122, a Puppet manifest update), the same engineer authored the change, self-approved the PR, and deployed to production. GitHub branch protection for the infrastructure repository did not enforce required reviewers. |
| **Evidence Reference** | GitHub branch protection rules; PR audit log; ArgoCD deployment audit |
| **Deficiency** | Yes — see finding **CM-F-04** |

---

## Domain 3: Backup and Recovery (B&R)

### BR-01 — Backup Scheduling and Coverage

| Attribute | Detail |
|---|---|
| **Procedure ID** | **BR-01** |
| **Control Objective** | All production data and system configurations are backed up according to defined schedules and retention policies. |
| **Risk Rating** | **High** |
| **COBIT Reference** | DSS04.01 — Define backup and recovery requirements |
| **Test Steps** | 1. Review the backup policy document for defined schedules, retention periods, and coverage requirements. 2. Obtain the AWS Backup vault inventory and verify all production RDS instances, EBS volumes, and S3 buckets containing PHI are included in backup plans. 3. Verify DynamoDB table backup configuration (point-in-time recovery enabled). 4. Confirm retention periods match policy (daily — 30 days, weekly — 12 weeks, monthly — 12 months). |
| **Expected Result** | All PHI-containing resources covered by automated backup plans with retention matching policy. |
| **Actual Result** | 42 of 44 in-scope resources are covered. Two unencrypted EBS volumes in the us-west-2 region used by a legacy analytics pipeline were not included in any backup plan. These volumes contained de-identified PHI-derived datasets. |
| **Evidence Reference** | AWS Backup vault inventory (snapshot Apr 2026); Backup policy v2.3; Resource tagging report (PHI-tagged resources) |
| **Deficiency** | Yes — see finding **BR-F-01** |

---

### BR-02 — Restoration Testing

| Attribute | Detail |
|---|---|
| **Procedure ID** | **BR-02** |
| **Control Objective** | Backups are periodically restored and tested to verify data integrity and recovery time objectives (RTO). |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | DSS04.03 — Test backup and recovery |
| **Test Steps** | 1. Obtain the restoration test schedule and results for the audit period. 2. Verify at least one full restoration test was performed per quarter. 3. Review restoration test reports for documented pass/fail criteria, RTO measurement, and data integrity validation. 4. Select a sample of 3 restoration tests and independently verify that restored data is accessible and consistent. |
| **Expected Result** | Quarterly restoration tests completed with documented pass results; RTO within 4 hours; data integrity validated. |
| **Actual Result** | Q1 restoration test was completed successfully (RDS snapshot restore, 3.2 hours RTO). Q2 restoration test was attempted but failed due to a misconfigured IAM role in the DR account. Restoration was completed using an alternate method after 7 hours. The failure was documented, but no root cause analysis was performed. |
| **Evidence Reference** | Restoration test reports Q1-2026, Q2-2026; AWS CloudTrail logs for DR account; incident ticket ITSEC-4012 |
| **Deficiency** | Yes — see finding **BR-F-02** |

---

### BR-03 — Off-Site Storage / Geographic Redundancy

| Attribute | Detail |
|---|---|
| **Procedure ID** | **BR-03** |
| **Control Objective** | Backup data is stored in a geographically separate location from the primary production environment to protect against site-level failures. |
| **Risk Rating** | **Low** |
| **COBIT Reference** | DSS04.01 — Define backup and recovery requirements |
| **Test Steps** | 1. Review the backup architecture documentation. 2. Verify AWS Backup cross-region copy settings. 3. Confirm that secondary region (us-west-2) backup vaults contain copies of primary region (us-east-1) backups. 4. Check cross-region replication schedule and encryption status. |
| **Expected Result** | Cross-region backup copies enabled for all critical PHI datasets; replication completes within 24 hours; encryption at rest enabled in secondary region. |
| **Actual Result** | Cross-region replication is configured for RDS and S3 backups. However, DynamoDB point-in-time recovery backups are not replicated across regions. The secondary region vault uses the same KMS key as the primary, creating a single-point-of-failure risk if the key is compromised in the primary region. |
| **Evidence Reference** | AWS Backup cross-region replication configuration; KMS key policy; DynamoDB backup settings |
| **Deficiency** | Yes — see finding **BR-F-03** |

---

### BR-04 — Backup Monitoring and Alerting

| Attribute | Detail |
|---|---|
| **Procedure ID** | **BR-04** |
| **Control Objective** | Backup failures and anomalies are detected, alerted, and resolved within defined SLAs. |
| **Risk Rating** | **Low** |
| **COBIT Reference** | DSS04.07 — Maintain and monitor the backup log |
| **Test Steps** | 1. Review backup monitoring dashboard in Datadog. 2. Verify alerts are configured for backup failures, missed backups, and incomplete backups. 3. Sample 30 days of backup job logs and check for failures. 4. Where failures exist, verify alert was triggered and resolved within SLA (4 hours). |
| **Expected Result** | Alerts configured for all failure conditions; failures in sampled period have documented alert and resolution. |
| **Actual Result** | Backups are monitored via Datadog with alerts for job failures. In the sampled 30-day period, three backup failures occurred (two due to AWS service limits, one due to an expired IAM key). All three triggered alerts and were resolved within SLA. No missed backups were identified. |
| **Evidence Reference** | Datadog backup monitoring dashboard; alert history (March–April 2026); incident tickets ITSEC-3890, ITSEC-3921, ITSEC-3988 |
| **Deficiency** | No — control is operating effectively |

---

## Domain 4: Segregation of Duties (SoD)

### SOD-01 — Segregation of Duties Matrix

| Attribute | Detail |
|---|---|
| **Procedure ID** | **SOD-01** |
| **Control Objective** | A formal Segregation of Duties matrix has been defined and is maintained to identify incompatible role combinations. |
| **Risk Rating** | **Low** |
| **COBIT Reference** | DSS06.02 — Segregation of duties |
| **Test Steps** | 1. Obtain the SoD matrix document and verify it covers all in-scope systems. 2. Review the defined incompatible combinations (e.g., developer + production admin, change approver + deployer). 3. Verify the matrix is reviewed annually. 4. Assess whether the matrix aligns with COBIT 2019 guidance on incompatible duties. |
| **Expected Result** | Comprehensive SoD matrix exists, covers all systems, is current (reviewed within 12 months), and includes all expected incompatible combinations. |
| **Actual Result** | An SoD matrix exists and covers AWS, GitHub, and Okta. However, it has not been updated since November 2024 (18 months stale). The matrix does not address combinations involving the new Datadog and PagerDuty platforms. Two incompatible combinations identified during the audit (GitHub admin + Jira admin, and AWS network admin + security group manager) are not documented. |
| **Evidence Reference** | SoD matrix v1.2 (Nov 2024); System architecture inventory; Role definitions for Datadog and PagerDuty |
| **Deficiency** | Yes — see finding **SOD-F-01** |

---

### SOD-02 — Incompatible Combination Review

| Attribute | Detail |
|---|---|
| **Procedure ID** | **SOD-02** |
| **Control Objective** | User access rights are analysed on a quarterly basis to identify and remediate incompatible combinations of entitlements. |
| **Risk Rating** | **High** |
| **COBIT Reference** | APO07.05 — Define and communicate roles and responsibilities |
| **Test Steps** | 1. Obtain the quarterly SoD analysis reports (user entitlement cross-match) for Q1 and Q2 2026. 2. Verify the analysis covers all in-scope systems. 3. Identify any users flagged for incompatible combinations. 4. Review remediation actions taken on flagged users. 5. Re-perform the analysis using current entitlement data to validate completeness. |
| **Expected Result** | Quarterly SoD analysis performed; flagged incompatible combinations remediated or approved with compensating controls. |
| **Actual Result** | No formal automated SoD analysis has been performed during the audit period. The audit team's re-performance of an entitlement cross-match identified 12 users with potentially incompatible role combinations (e.g., developers with production database write access, infrastructure engineers with both GitHub admin and AWS admin). None had compensating controls documented. |
| **Evidence Reference** | Access entitlement exports (AWS IAM, GitHub, Okta) as of Apr 2026; custom SQL cross-match analysis; stakeholder interview notes |
| **Deficiency** | Yes — see finding **SOD-F-02** |

---

### SOD-03 — Access Violation Monitoring

| Attribute | Detail |
|---|---|
| **Procedure ID** | **SOD-03** |
| **Control Objective** | Access violations and security events are monitored, logged, and reviewed to detect potential segregation of duties breaches. |
| **Risk Rating** | **Medium** |
| **COBIT Reference** | MEA03.01 — Monitor compliance with internal policies |
| **Test Steps** | 1. Review the SIEM (Datadog — Security Monitoring) configuration for access violation rules. 2. Sample 15 security alerts generated during the audit period related to access violations. 3. Verify each alert was reviewed, triaged, and resolved within SLA. 4. Assess whether alert rules cover SoD-related patterns (e.g., user with admin roles in two systems performing cross-system actions). |
| **Expected Result** | Access violation alerts are configured, consistently triaged, and resolved within defined SLA. |
| **Actual Result** | Datadog Security Monitoring is configured for access violation detection. All 15 sampled alerts were triaged within SLA. However, detection rules focus on infrastructure-level violations (e.g., S3 public access) and do not include SoD-specific patterns (e.g., a single user performing privileged actions across multiple systems). |
| **Evidence Reference** | Datadog Security Monitoring rules; 15 sampled incident tickets; SIEM configuration review |
| **Deficiency** | Yes — see finding **SOD-F-03** |
