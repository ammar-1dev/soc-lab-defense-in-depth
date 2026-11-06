# Control Framework Cross-Reference

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-006` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Objective and Status of This Mapping

This document cross-references the implemented laboratory controls to three published frameworks: **ISO/IEC 27001:2022 Annex A**, **PCI DSS v4.0** and **CIS Controls v8.1**.

> **Important qualification.** This is an **informative alignment mapping produced in a laboratory context**. It is not a statement of applicability, not a compliance assessment, and not a certification claim. No audit was performed. No cardholder data environment exists. The mapping indicates which published control objectives the implemented technical measures correspond to — nothing more.

---

## 2. ISO/IEC 27001:2022 Annex A

| Annex A control | Title | Laboratory implementation | Layer |
|:---|:---|:---|:---:|
| `A.5.15` | Access control | Domain authentication and group-based authorisation | 2 |
| `A.5.24` | Information security incident management planning and preparation | Documented triage and response procedure | 7 |
| `A.5.25` | Assessment and decision on information security events | Severity assignment and correlation logic | 6 |
| `A.5.26` | Response to information security incidents | Automated containment and escalation runbook | 7 |
| `A.7.10` | Storage media | Removable-media access restriction | 3 |
| `A.8.1` | User endpoint devices | Endpoint hardening and application control | 3 |
| `A.8.2` | Privileged access rights | Privileged-group monitoring and critical alerting | 5 |
| `A.8.7` | Protection against malware | Anti-malware control validation via test artefact | 3 |
| `A.8.9` | Configuration management | Centralised endpoint configuration management | 4 |
| `A.8.15` | Logging | Central forwarding and indexing of security events | 6 |
| `A.8.16` | Monitoring activities | Cross-source correlation and alerting | 6 |
| `A.8.17` | Clock synchronisation | **Open item** — see limitations register | — |
| `A.8.19` | Installation of software on operational systems | Application allow-listing | 3 |
| `A.8.20` | Networks security | Perimeter policy enforcement | 1 |
| `A.8.22` | Segregation of networks | Three-zone segmentation with single inspection path | 1 |

### 2.1 Controls explicitly not addressed

Annex A contains 93 controls across four themes. The laboratory addresses a subset of the technological theme and a small number of organizational controls. Physical, people and most organizational controls are outside the scope of a technical laboratory and are neither implemented nor claimed.

---

## 3. PCI DSS v4.0

| Requirement | Title | Laboratory correspondence |
|:---:|:---|:---|
| `1` | Install and Maintain Network Security Controls | Firewall policy and zone segmentation |
| `5` | Protect All Systems and Networks from Malicious Software | Endpoint protection and control validation |
| `7` | Restrict Access by Business Need to Know | Group-based authorisation |
| `8` | Identify Users and Authenticate Access | Domain authentication and lockout policy |
| `10` | Log and Monitor All Access | Central logging, correlation and alerting |
| `11` | Test Security of Systems and Networks Regularly | Authorised adversary emulation and detection validation |
| `12` | Support Information Security with Organizational Policies and Programs | Documented runbook and documentation set |

> No cardholder data is stored, processed or transmitted in this environment. The mapping above is illustrative of control correspondence only.

---

## 4. CIS Controls v8.1

| Control | Title | Laboratory correspondence | Implementation depth |
|:---:|:---|:---|:---|
| `1` | Inventory and Control of Enterprise Assets | Documented host inventory (`01-architecture.md`, §5) | Partial |
| `2` | Inventory and Control of Software Assets | Application allow-list baseline | Partial |
| `3` | Data Protection | Removable-media restriction | Partial |
| `4` | Secure Configuration of Enterprise Assets and Software | Centralised configuration management | Partial |
| `5` | Account Management | Domain account lifecycle and group governance | Partial |
| `6` | Access Control Management | Group-based authorisation, privileged-group monitoring | Partial |
| `8` | Audit Log Management | Central collection and indexing | Substantial |
| `10` | Malware Defenses | Endpoint protection and validation | Partial |
| `12` | Network Infrastructure Management | Firewall administration and zone design | Partial |
| `13` | Network Monitoring and Defense | Inline inspection and custom signatures | Substantial |
| `17` | Incident Response Management | Triage and response runbook | Partial |
| `18` | Penetration Testing | Authorised scenario-based validation | Partial |

### 4.1 Safeguards of particular relevance

| Safeguard | Title | Status in this laboratory |
|:---|:---|:---|
| `8.2` | Collect Audit Logs | Implemented — syslog and forwarder ingestion |
| `8.4` | Standardize Time Synchronization | **Open item** |
| `8.5` | Collect Detailed Audit Logs | Implemented — Sysmon and advanced audit policy |
| `8.9` | Centralize Audit Logs | Implemented — central indexer |
| `8.10` | Retain Audit Logs | **Open item** — retention sizing not formalised |
| `8.11` | Conduct Audit Log Reviews | Partial — review performed during validation, not on a recurring cadence |
| `13.11` | Tune Security Event Alerting Thresholds | Implemented — rate limiting per authoring rule `DE-04` |

---

## 5. MITRE ATT&CK

Technique-level mapping is maintained in [`04-validation-and-results.md`](04-validation-and-results.md), §9, which distinguishes **validated** techniques from techniques with **signature coverage only**. That distinction shall be preserved in any reuse of this mapping.

---

## 6. Consolidated Gap Register

| Ref | Gap | Framework references | Treatment |
|:---|:---|:---|:---|
| `G-01` | Time synchronisation not formalised | ISO `A.8.17` · CIS `8.4` | Roadmap item |
| `G-02` | Log retention not sized or documented | CIS `8.10` | Roadmap item |
| `G-03` | Log review not on a recurring cadence | CIS `8.11` | Roadmap item |
| `G-04` | Asset inventory not maintained as a live register | CIS `1`, `2` | Roadmap item |
| `G-05` | No vulnerability management cycle | CIS `7` | Out of laboratory scope |
| `G-06` | No encrypted-traffic inspection | ISO `A.8.20` | Accepted limitation |

---

## 7. Reuse Guidance

Where this mapping is used as a starting point for a real statement of applicability:

1. Each control **shall** be assessed against the actual organisational context and risk assessment.
2. Exclusions **shall** be justified explicitly, as required by ISO/IEC 27001 clause 6.1.3.
3. Laboratory evidence **shall not** be presented as production evidence.
4. Any compliance assertion **shall** be supported by an independent assessment, not by this document.

---

[← Validation and results](04-validation-and-results.md) · [Operations runbook →](06-operations-runbook.md)
