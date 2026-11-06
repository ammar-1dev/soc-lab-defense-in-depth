# Layered Control Design

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-003` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Objective

This document specifies the seven defence layers, the control objective of each, the mechanism by which the objective is achieved, and the residual risk that remains after the layer is applied.

The design follows the defence-in-depth principle: each layer assumes that the preceding layer may fail, and no single layer is relied upon to achieve a control objective on its own.

---

## 2. Layer Model

| Layer | Domain | Primary function | Control class |
|:---:|:---|:---|:---|
| 1 | Perimeter | Filter and inspect crossing traffic | Preventive + Detective |
| 2 | Identity | Govern authentication and authorisation | Preventive |
| 3 | Endpoint | Constrain execution and device usage | Preventive + Detective |
| 4 | Management | Maintain configuration and patch state | Preventive |
| 5 | Audit | Record and review directory change | Detective |
| 6 | Visibility | Correlate telemetry across sources | Detective |
| 7 | Response | Contain confirmed activity | Responsive |

---

## 3. Layer 1 — Perimeter

**Control objective.** Only explicitly permitted traffic shall cross a trust-zone boundary, and all crossing traffic shall be subject to signature inspection.

**Mechanism.** pfSense enforces interface-level policy. Snort operates on the firewall interfaces and raises alerts on signature match. Custom signatures are layered above the subscribed public rulesets.

**Design decision.** Default-deny between `UNTRUST` and `LAN`, with narrowly scoped permits, means that even a successful scan yields limited actionable information.

**Residual risk.** Encrypted payloads are not inspected. East–west traffic within `LAN` does not traverse this layer.

**Evidence.** Figure 2 (interface assignments), Figure 3 (inspection configuration).

---

## 4. Layer 2 — Identity

**Control objective.** Access to domain resources shall be authenticated, authorised by group membership, and governed by centrally enforced policy.

**Mechanism.** Active Directory Domain Services under `corp.test`, with **seven Group Policy Objects** applied to the relevant organisational units.

### 4.1 Policy objectives evidenced by validation

| Policy objective | Enforced behaviour | Validated by |
|:---|:---|:---|
| Removable-media access restriction | Write operations to removable storage are denied | Scenario 3 |
| Application control | Execution outside the permitted set is denied | Scenario 4 |
| Advanced audit policy | Object access and logon events are recorded | Scenarios 2, 3, 5 |
| Account lockout and password policy | Repeated authentication failure is constrained | Scenario 2 |

The complete Group Policy export is listed as a pending artefact in `07-limitations-and-roadmap.md`. Only the four objectives above are presented as validated in this edition.

**Residual risk.** A valid credential used within its permitted scope produces no policy violation; behavioural analytics is required to detect misuse of legitimate access.

---

## 5. Layer 3 — Endpoint

**Control objective.** Execution and device usage on the endpoint shall be restricted to an approved baseline, and security-relevant activity shall be recorded with sufficient detail for reconstruction.

**Mechanism.**

| Component | Function |
|:---|:---|
| AppLocker | Application allow-listing; denials recorded as event `8004` |
| Removable-storage policy | Device-class access restriction; access attempts recorded as event `4663` |
| Sysmon | Process, network and image-load telemetry beyond the default audit policy |

**Design decision.** Application control was configured to deny rather than to audit-only. This converts the control from detective to preventive, while retaining the audit record required for scoping.

**Residual risk.** Allow-list bypass techniques and in-memory execution are not fully covered by application allow-listing alone.

---

## 6. Layer 4 — Management

**Control objective.** Endpoint configuration and patch state shall be centrally administered so that control drift is detectable.

**Mechanism.** Centralised endpoint administration provides inventory, configuration deployment and update orchestration across managed hosts.

**Residual risk.** The management plane is itself a high-value target; compromise of the management server permits estate-wide configuration change. Access to it shall be restricted to the corporate segment and treated as privileged.

---

## 7. Layer 5 — Audit

**Control objective.** Changes to directory objects — in particular to privileged group membership — shall be recorded and shall raise an alert without analyst intervention.

**Mechanism.** Directory change auditing produces alerts on security-enabled group modification, correlated with Windows Security event `4728`.

**Design decision.** Privileged-group modification was assigned critical severity irrespective of the initiating account, because the action is rare in a stable directory and high in impact.

**Residual risk.** An attacker who clears the local event log removes the host copy; centralised forwarding under Layer 6 is the compensating control.

---

## 8. Layer 6 — Visibility

**Control objective.** Telemetry from network, identity and endpoint sources shall be centrally indexed and correlated so that a single event can be evaluated in context.

**Mechanism.**

| Platform | Contribution |
|:---|:---|
| Splunk Enterprise | Central indexing, cross-source correlation, dashboards and alerting |
| Wazuh | Host-based detection, file-integrity monitoring, agent telemetry |

**Design decision.** Network alerts and Windows event channels are indexed separately but queried jointly. This preserves source fidelity while enabling correlation — for example, a Snort RDP signature matched against a burst of `4625` events on the targeted host.

**Residual risk.** Correlation quality depends on consistent timestamps and complete forwarding. A silently failing forwarder produces false assurance rather than an visible gap; forwarder health monitoring is therefore required.

**Evidence.** Figure 4 (index management view).

---

## 9. Layer 7 — Response

**Control objective.** Confirmed malicious activity shall be contained automatically where the confidence of the detection is high, and by documented procedure otherwise.

**Mechanism.**

| Trigger | Automated action |
|:---|:---|
| High-confidence network signature | Source address blocked at the firewall |
| Host-based detection meeting response criteria | Wazuh active response executes the configured containment action |
| Policy violation on the endpoint | Execution or device access denied by the applied Group Policy |

**Design decision.** Automated blocking is restricted to high-confidence signatures. Broad automated blocking on low-fidelity detections creates a self-inflicted denial-of-service condition and was deliberately avoided.

**Residual risk.** Automated blocking is address-based and is therefore ineffective against a rotating source. Manual escalation procedures are defined in `06-operations-runbook.md`.

---

## 10. Layer Interdependence

| Layer | Depends on | Failure consequence if dependency is absent |
|:---:|:---|:---|
| 6 | 1, 3, 5 | No telemetry to correlate; the SIEM indexes nothing of value |
| 7 | 6 | Containment has no validated trigger and becomes arbitrary |
| 5 | 2 | Audit policy is not applied; group changes go unrecorded |
| 3 | 4 | Configuration drift silently disables endpoint controls |

This dependency chain is the technical substance of the project's principal finding: the layers are not seven independent products but one integrated system.

---

## 11. Control Coverage Against Validated Scenarios

| Scenario | Prevented by | Detected by | Correlated in |
|:---:|:---|:---|:---|
| 1 | — | Layer 1 | Layer 6 |
| 2 | Layer 2 (lockout) | Layers 1, 5 | Layer 6 |
| 3 | Layers 2, 3 | Layer 3 | Layer 6 |
| 4 | Layers 2, 3 | Layers 3, 6 | Layer 6 |
| 5 | — | Layer 5 | Layer 6 |

Scenarios 1 and 5 were detected but not prevented. This is the expected and correct outcome: reconnaissance and a legitimate administrative action cannot be blocked by policy without impairing normal operations, so detection and response carry the control objective.

---

[← Architecture](01-architecture.md) · [Detection engineering →](03-detection-engineering.md)
