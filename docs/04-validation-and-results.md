# Validation Plan and Results

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-005` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Objective

This document records the validation methodology, the acceptance criteria declared before execution, the five test cases, their outcomes and the mapping of each case to the MITRE ATT&CK Enterprise knowledge base.

---

## 2. Methodology

### 2.1 Principle

Validation was conducted as **measurement, not demonstration**. For each test case the expected detection artefact was declared *before* the activity was executed. A case passes only if the declared artefact is observed; a control that merely exists in configuration does not constitute a pass.

### 2.2 Procedure

| Step | Activity |
|:---:|:---|
| 1 | Define the scenario and select the corresponding ATT&CK technique |
| 2 | Declare the expected artefact: source, event identifier or SID, and severity |
| 3 | Confirm baseline telemetry is flowing and the collector is healthy |
| 4 | Execute the activity from the `UNTRUST` segment |
| 5 | Record whether the declared artefact was produced, and where it was observed |
| 6 | Record whether the activity was additionally prevented |
| 7 | Document any gap between expectation and observation |

### 2.3 Acceptance criteria

| Ref | Criterion |
|:---|:---|
| `AC-01` | The declared artefact **shall** be present in the central collector, not only on the source host. |
| `AC-02` | The artefact **shall** be attributable to the correct source and target. |
| `AC-03` | Where a preventive control applies, the activity **shall** be denied. |
| `AC-04` | Prevention **shall not** suppress the audit record of the attempt. |
| `AC-05` | Any deviation from expectation **shall** be recorded as a finding, whether or not it is favourable. |

### 2.4 Authorisation and containment

All activity originated from `10.10.40.0/24`, a host-only network with no route to external systems. Targets were laboratory hosts created by the project team for this purpose. No third-party system, public address or third-party data was involved.

---

## 3. Test Case TC-01 — TCP SYN Service Discovery

| Field | Value |
|:---|:---|
| **ATT&CK technique** | [T1046 — Network Service Discovery](https://attack.mitre.org/techniques/T1046/) |
| **Tactic** | Discovery |
| **Origin** | Adversary platform, `UNTRUST` |
| **Target** | Corporate segment address range |
| **Expected artefact** | Snort signature in the custom scan set, forwarded to the network detection index |
| **Preventive control** | None — reconnaissance is not blocked by policy |

**Outcome — PASS.** The scan produced the expected network signature, and the alert was correlated in the central console. Related signatures for XMAS and NULL flag patterns (`1000003`, `1000004`) provide coverage of alternative scan types.

**Observation.** Detection without prevention is the correct outcome for this case. Blocking all inbound connection attempts would also block legitimate service access; the control objective is therefore assigned to detection and response.

---

## 4. Test Case TC-02 — RDP Credential Guessing

| Field | Value |
|:---|:---|
| **ATT&CK technique** | [T1110.001 — Brute Force: Password Guessing](https://attack.mitre.org/techniques/T1110/001/) |
| **Tactic** | Credential Access |
| **Origin** | Adversary platform, `UNTRUST` |
| **Target** | Remote desktop service on a corporate host |
| **Expected artefacts** | Snort `1000005`; Windows Security `4625`; directory audit alert |
| **Preventive control** | Account lockout policy |

**Outcome — PASS.** All three declared artefacts were observed. The network signature identified the attempt in transit, the host event channel confirmed that authentication failures reached the target, and directory auditing raised the corresponding alert.

**Observation.** This case demonstrates the integration principle most directly. The network signature alone cannot establish whether guessing succeeded; the host events alone cannot establish that the attempt originated outside the zone. Only the join establishes both.

---

## 5. Test Case TC-03 — Removable-Media Exfiltration Attempt

| Field | Value |
|:---|:---|
| **ATT&CK technique** | [T1052.001 — Exfiltration Over Physical Medium: Exfiltration over USB](https://attack.mitre.org/techniques/T1052/001/) |
| **Tactic** | Exfiltration |
| **Origin** | Corporate endpoint |
| **Target** | Removable storage device |
| **Expected artefacts** | Policy denial; Windows Security `4663` |
| **Preventive control** | Removable-storage access restriction via Group Policy |

**Outcome — PASS.** The write operation was denied by policy and the access attempt was recorded as event `4663`. Criterion `AC-04` was satisfied: prevention did not suppress the audit record.

**Observation.** A preventive control that produces no record is operationally incomplete — the attempt would have been invisible to the analyst. Retaining the denial event is what makes the control useful for scoping.

---

## 6. Test Case TC-04 — Unauthorised Application Execution

| Field | Value |
|:---|:---|
| **ATT&CK technique** | [T1204.002 — User Execution: Malicious File](https://attack.mitre.org/techniques/T1204/002/) |
| **Tactic** | Execution |
| **Origin** | Corporate endpoint |
| **Target** | Application outside the approved set |
| **Expected artefacts** | AppLocker `8004`; Wazuh alert |
| **Preventive control** | Application allow-listing |

**Outcome — PASS.** Execution was denied and recorded as event `8004`; the host detection platform raised the corresponding alert.

**Observation.** Allow-listing was configured to deny rather than audit-only. The audit record was retained, satisfying `AC-04` and preserving the ability to identify which file was attempted.

---

## 7. Test Case TC-05 — Privilege Escalation via Group Membership

| Field | Value |
|:---|:---|
| **ATT&CK technique** | [T1098 — Account Manipulation](https://attack.mitre.org/techniques/T1098/) |
| **Tactic** | Persistence · Privilege Escalation |
| **Origin** | Authenticated administrative context |
| **Target** | Security-enabled global group |
| **Expected artefacts** | Windows Security `4728`; critical directory audit alert |
| **Preventive control** | None — the action is legitimate when authorised |

**Outcome — PASS.** The membership change produced event `4728` and a critical-severity directory audit alert identifying the initiating account.

**Observation.** This case cannot be prevented by policy without impairing legitimate administration. Detection severity was therefore set to critical on the basis of rarity and impact rather than on the basis of the initiating account's authority — a legitimate-looking account performing a rare, high-impact action is precisely the pattern that warrants review.

---

## 8. Results Summary

| Case | Technique | Expected artefact observed | Additionally prevented | Result |
|:---:|:---|:---:|:---:|:---:|
| TC-01 | T1046 | Yes | No | **PASS** |
| TC-02 | T1110.001 | Yes | Partially (lockout) | **PASS** |
| TC-03 | T1052.001 | Yes | Yes | **PASS** |
| TC-04 | T1204.002 | Yes | Yes | **PASS** |
| TC-05 | T1098 | Yes | No | **PASS** |

**5 of 5 cases passed.** More than twenty distinct alert classes were observed across the run.

---

## 9. ATT&CK Coverage Matrix

| Tactic | Technique covered | Validated | Signature coverage only |
|:---|:---|:---:|:---:|
| Discovery | T1046 | ✔ | ✔ |
| Credential Access | T1110.001 | ✔ | ✔ |
| Lateral Movement | T1021.002 | — | ✔ |
| Execution | T1204.002 | ✔ | — |
| Persistence / Privilege Escalation | T1098 | ✔ | — |
| Command and Control | T1071.004 | — | ✔ |
| Exfiltration | T1052.001 · T1048 | ✔ (T1052.001) | ✔ (T1048) |
| Defence Evasion | T1562.001 | — | ✔ |
| Credential Access (network exposure) | T1040 | — | ✔ |

**Coverage note.** A technique marked *signature coverage only* has detection logic deployed but was not exercised in this validation run. It shall not be reported as validated. Extending execution to these techniques is the first item on the roadmap.

---

## 10. Findings

| Ref | Finding | Classification |
|:---|:---|:---|
| `F-01` | Detection capability arises from correlation across sources, not from any single tool | Design conclusion |
| `F-02` | Control gaps were identified only through authorised adversary execution, not through configuration review | Process conclusion |
| `F-03` | Preventive controls must retain the audit record of the denied attempt | Configuration requirement |
| `F-04` | Declaring expected artefacts before execution converts a demonstration into a measurement | Methodology conclusion |
| `F-05` | Nine techniques have coverage; five were exercised. Coverage breadth exceeds validated breadth and must be reported separately | Reporting requirement |

---

## 11. Reproducibility

The scenarios are described at a level sufficient to reproduce them in an equivalent isolated laboratory. Specific tooling invocations and payload details are intentionally omitted: the purpose of this repository is to document defensive design and detection outcomes, not to distribute an execution guide.

---

[← Detection engineering](03-detection-engineering.md) · [Control framework mapping →](05-control-framework-mapping.md)
