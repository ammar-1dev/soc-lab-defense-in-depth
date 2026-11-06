# Limitations and Roadmap

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-008` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Purpose

This document states, without mitigation, what the laboratory does not do, what has not been verified, and what is planned. It exists so that no reader mistakes design intent for validated capability.

---

## 2. Architectural Limitations

| Ref | Limitation | Consequence | Status |
|:---|:---|:---|:---|
| `L-01` | All zones reside on one hypervisor | Hypervisor compromise defeats segmentation | Accepted — inherent to a single-host laboratory |
| `L-02` | Collector and directory share the corporate segment | A compromised endpoint is network-adjacent to the log store | Accepted; dedicated management zone recommended for production |
| `L-03` | No high-availability or redundancy | Loss of the collector interrupts all detection | Accepted — availability engineering is out of scope |
| `L-04` | No encrypted-traffic inspection | Payload signatures do not apply to TLS sessions | Accepted; metadata-based detection is the alternative |
| `L-05` | Scale is six hosts | Findings do not extrapolate to enterprise volume | Accepted — stated explicitly to prevent over-generalisation |

---

## 3. Detection Limitations

| Ref | Limitation | Consequence |
|:---|:---|:---|
| `L-06` | Baseline observation period is short | Low-and-slow activity may not deviate detectably from baseline |
| `L-07` | Automated blocking is address-based | Ineffective against a rotating source |
| `L-08` | Application allow-listing does not cover in-memory execution | Requires memory-oriented telemetry not deployed here |
| `L-09` | Nine techniques have coverage; five were exercised | Coverage breadth exceeds validated breadth |
| `L-10` | Time synchronisation is not formalised | Correlation across sources depends on consistent timestamps |
| `L-11` | Forwarder health is not monitored | A silent forwarder failure produces false assurance |

`L-09` is a reporting constraint as much as a technical one: techniques with signature coverage only **shall not** be described as validated.

---

## 4. Process Limitations

| Ref | Limitation | Consequence |
|:---|:---|:---|
| `L-12` | Log retention is not sized or documented | Investigative lookback is undefined |
| `L-13` | Log review has no recurring cadence | Review occurred during validation only |
| `L-14` | Asset inventory is a document, not a live register | Drift is not automatically detected |
| `L-15` | Runbook not tested under concurrent incidents | Multi-incident prioritisation is unproven |
| `L-16` | No on-call rotation or escalation timetable | Not applicable to a project-team laboratory |
| `L-17` | No vulnerability management cycle | Out of laboratory scope |

---

## 5. Pending Artefacts

The following are referenced as planned work. They are **not present** in this repository and are not claimed as evidence.

| Artefact | Intended path | Blocker | Priority |
|:---|:---|:---|:---:|
| Raw custom signature file | `rules/local.rules` | Requires address sanitisation and a final review against `DE-02` | High |
| Alert-time captures | `docs/alerts/` | Requires re-execution of the scenarios with capture enabled | High |
| Second page of the index list | `docs/screenshots/04-splunk-custom-indexes.png` | Capture outstanding | Medium |
| Scenario write-ups | `scenarios/` | Requires redaction of tooling invocations | Medium |
| Group Policy export | `configs/gpo/` | Requires export and removal of organisational detail | Medium |
| Index definitions | `configs/splunk/` | Requires configuration export | Low |
| Full project report | `docs/report.pdf` | Requires an English edition consistent with this set | Low |

> The repository documents completed, verified work only. This table is the sole place where unfinished work is described, and it is labelled as such.

---

## 6. Roadmap

### 6.1 Phase 1 — Close the evidence gap

| Item | Objective |
|:---|:---|
| Publish the sanitised signature file | Make the detection logic independently reviewable |
| Capture alert-time evidence | Replace configuration evidence with detection evidence |
| Document all five scenarios | Make the validation reproducible by a third party |

### 6.2 Phase 2 — Extend validated coverage

| Item | Objective |
|:---|:---|
| Exercise the four signature-only techniques | Convert coverage into validated detection |
| Add a lateral-movement scenario | Test east–west visibility, the weakest architectural path |
| Add a log-tampering scenario | Test whether central forwarding survives host log clearing |

### 6.3 Phase 3 — Operational maturity

| Item | Objective |
|:---|:---|
| Formalise time synchronisation | Close `L-10` and CIS Safeguard 8.4 |
| Define retention sizing | Close `L-12` and CIS Safeguard 8.10 |
| Add forwarder health monitoring | Close `L-11` |
| Establish a recurring review cadence | Close `L-13` and CIS Safeguard 8.11 |

---

## 7. Explicit Non-Goals

The following are deliberately excluded and are not planned:

- Production deployment or hardened reference-architecture status
- Redistribution of third-party ruleset content
- Publication of an executable attack guide
- Any compliance certification claim
- Testing against any system not owned by the project team

---

## 8. Statement of Integrity

Every capability described elsewhere in this documentation set was either observed during the validation run or is evidenced by an artefact present in this repository. Items that are designed but unverified appear only in this document. Where a figure does not evidence the claim it accompanies, the figure caption states so.

---

[← Operations runbook](06-operations-runbook.md) · [Glossary →](08-glossary.md)
