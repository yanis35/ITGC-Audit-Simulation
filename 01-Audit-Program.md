# Audit Planning Document — ITGC Baseline Assessment

| Field | Value |
|---|---|
| **Document ID** | AP-MT-2026-001 |
| **Version** | 1.0 |
| **Date** | December 15, 2025 |
| **Status** | Approved |
| **Prepared By** | Michael Torres, CIA — Internal Audit Lead |
| **Approved By** | Dr. Elena Vasquez — Chief Information Officer |

---

## Engagement Memo

### Audit Objective

To assess the design and operating effectiveness of MediTrust Health's IT General Controls across the four domains defined below, establish a baseline control posture in preparation for the SOC 2 Type II audit, and identify control deficiencies requiring remediation.

### Scope

| Parameter | Scope Detail |
|---|---|
| **In-Scope Systems** | MediTrust Core Platform (PHI-processing SaaS), AWS production environment, Okta SSO, Jira Service Management, GitHub Enterprise, Datadog |
| **In-Scope Locations** | Headquarters (Boston, MA) and AWS us-east-1 / us-west-2 |
| **Out of Scope** | Third-party data processors (covered under separate TPRM workstream), corporate finance systems, HRIS |
| **Audit Period** | January 1, 2026 – June 30, 2026 |
| **Control Framework** | COBIT 2019 (Governance and Management Objectives) |

### Timeline

| Phase | Start | End | Deliverable |
|---|---|---|---|
| Planning | Dec 15, 2025 | Jan 10, 2026 | Audit Program Document |
| Fieldwork — Domain 1 | Jan 13, 2026 | Feb 7, 2026 | Working Papers (UAM) |
| Fieldwork — Domain 2 | Feb 10, 2026 | Mar 7, 2026 | Working Papers (CM) |
| Fieldwork — Domain 3 | Mar 10, 2026 | Apr 4, 2026 | Working Papers (B&R) |
| Fieldwork — Domain 4 | Apr 7, 2026 | Apr 25, 2026 | Working Papers (SoD) |
| Reporting | Apr 28, 2026 | May 16, 2026 | Findings Register, CAP, Executive Summary |
| Remediation | May 19, 2026 | Nov 20, 2026 | CAP Closure Report |

---

## Entity-Level Risk Assessment Context

### Business Context

MediTrust Health operates a B2B SaaS platform that ingests, stores, and processes electronic protected health information (ePHI) on behalf of 15+ hospital networks. The platform supports clinical data exchange, patient identity management, and analytics. The company employs 300 staff across engineering, product, sales, and corporate functions.

### Key Risk Drivers

- **Regulatory Exposure**: HIPAA Omnibus Rule (45 CFR § 164.308–312) and state breach notification laws impose strict requirements on PHI access controls, audit logging, and breach response.
- **SOC 2 Readiness**: First formal audit — no prior ITGC baseline exists. Control gaps are expected and tolerable only with documented remediation.
- **Scale Velocity**: Engineering team doubled in the past 12 months (40 to 85 engineers); access provisioning and termination processes have not scaled proportionally.
- **Cloud Complexity**: Multi-region AWS deployment with dynamic infrastructure; change management and backup recovery processes span ephemeral resources.
- **Remote Workforce**: 100% remote engineering team increases reliance on SSO, device trust, and identity governance.

### Inherent Risk Assessment

| Risk Factor | Rating | Rationale |
|---|---|---|
| Regulatory Complexity | **High** | HIPAA + SOC 2 convergence; first-time audit |
| IT Environment Complexity | **High** | Multi-region cloud, containerized workloads, IaC |
| Organizational Change | **Medium** | Rapid engineering hiring; no IAM-dedicated resource |
| Prior Audit History | **High** | No formal ITGC audit ever performed |
| Third-Party Dependence | **Medium** | AWS as critical subcontractor; SOC 2 reports obtained |

---

## COBIT 2019 Control Objectives

The following COBIT 2019 governance and management objectives were selected as the control baseline for this engagement:

### Domain 1: User Access Management

| COBIT ID | Management Practice | Control Focus |
|---|---|---|
| **DSS05.04** | Manage access identities and enable access | User provisioning and de-provisioning |
| **DSS05.05** | Manage logical access | Authentication controls (MFA, SSO) |
| **APO13.01** | Establish and maintain an ISMS | Access recertification process |
| **DSS06.03** | Manage roles, responsibilities, and access privileges | Privileged access management |
| **APO07.06** | Manage employee changes | Termination access removal |

### Domain 2: Change Management

| COBIT ID | Management Practice | Control Focus |
|---|---|---|
| **BAI06.01** | Manage changes — change standards and procedures | Change request process |
| **BAI06.02** | Assess, prioritize, and authorize changes | Change approval workflow |
| **BAI06.03** | Manage emergency changes | Emergency change process |
| **BAI03.01** | Manage the identification and build of solutions | Testing & QA before production deployment |

### Domain 3: Backup and Recovery

| COBIT ID | Management Practice | Control Focus |
|---|---|---|
| **DSS04.01** | Define backup and recovery requirements | Backup scheduling and coverage |
| **DSS04.02** | Perform backups | Automated backup execution |
| **DSS04.03** | Test backup and recovery | Restoration testing |
| **DSS04.07** | Maintain and monitor the backup log | Backup monitoring and alerting |

### Domain 4: Segregation of Duties

| COBIT ID | Management Practice | Control Focus |
|---|---|---|
| **DSS06.02** | Segregation of duties | SoD matrix and incompatible combinations |
| **APO07.05** | Define and communicate roles and responsibilities | Role-based access design |
| **MEA03.01** | Monitor compliance with internal policies | Access violation monitoring |
| **DSS06.04** | Manage security breaches and incidents | Mitigating controls for SoD conflicts |

> **Note on untested COBIT objectives:** BAI06.02 (Change approval workflow) and DSS06.04 (SoD conflict mitigating controls) were evaluated through process walkthroughs and are documented separately in the working papers. DSS04.02 (Automated backup execution) is covered by test procedures BR-01 through BR-04.

---

## Audit Procedures Overview

| Domain | No. of Procedures | Control Type | Test Methods |
|---|---|---|---|
| User Access Management | 5 | Detective / Preventive | Walkthrough, Inspection, Re-performance |
| Change Management | 4 | Preventive | Walkthrough, Inspection |
| Backup and Recovery | 4 | Detective | Inspection, Re-performance |
| Segregation of Duties | 3 | Preventive / Detective | Inspection, Data Analysis |
| **Total** | **16** | — | — |

---

## Team and Resourcing

| Role | Resource | Effort (Hours) |
|---|---|---|
| Engagement Partner / Audit Lead | Michael Torres, CIA | 120 |
| IT Audit Senior | Priya Nair, CISA | 180 |
| IT Audit Staff | David Kim | 240 |
| Subject Matter Expert (Security) | James Okonkwo, CISO (advisory) | 20 |
| Subject Matter Expert (Engineering) | Sarah Chen, VP Eng (advisory) | 15 |
| Data Analytics Support | Ravi Patel, Data Engineer | 30 |

**Total Estimated Effort**: 605 hours
