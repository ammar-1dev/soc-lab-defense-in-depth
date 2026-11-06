# Triage and Response Runbook

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-007` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Objective

This runbook defines the procedure applied when an alert is raised in the laboratory: how it is triaged, how severity is assigned, which correlation steps are performed, and when containment is authorised.

It follows the preparation → detection and analysis → containment, eradication and recovery → post-incident activity structure of NIST SP 800-61.

---

## 2. Severity Model

| Severity | Definition | Target response |
|:---|:---|:---|
| **Critical** | Confirmed privileged change, successful unauthorised execution, or evidence of data leaving the zone | Immediate analyst action |
| **High** | Repeated authentication failure against a single target, or a high-confidence network signature | Triage within the same working session |
| **Medium** | Policy denial with no indication of success, or reconnaissance activity | Triage in the next review cycle |
| **Low** | Informational policy or configuration event | Aggregate review |

### 2.1 Severity assignment rule

Severity is assigned on **impact and rarity**, not on the authority of the initiating account. A rare, high-impact action performed by an administrative account is treated as critical, because account authority is precisely what an attacker seeks to obtain.

---

## 3. General Triage Procedure

| Step | Action | Output |
|:---:|:---|:---|
| 1 | Identify the detection source and the exact rule or event identifier | Rule/event reference |
| 2 | Establish source, target and timestamp | Scope triple |
| 3 | Determine whether the activity was prevented, detected, or both | Outcome classification |
| 4 | Join to the complementary source using §4 | Corroborating evidence |
| 5 | Assign severity using §2 | Severity |
| 6 | Decide: close, monitor, or contain | Disposition |
| 7 | Record the disposition and the reasoning | Case note |

Step 3 is decisive. An alert on a **prevented** action and an alert on a **successful** action of the same type carry entirely different severity, and the distinction is only visible when the preventive control's outcome is checked.

---

## 4. Correlation Playbooks

### 4.1 Credential guessing (`1000005`, `1000007`, event `4625`)

| Step | Query intent |
|:---:|:---|
| 1 | Retrieve the network signature; record source address and target service |
| 2 | Retrieve `4625` events on the target host within the alert window |
| 3 | Count failures per account and per source |
| 4 | Check for any successful logon event following the failure burst |
| 5 | Confirm whether account lockout was applied |

**Escalate to critical** if any successful authentication follows the failure burst. Otherwise classify as high and confirm the lockout took effect.

### 4.2 Removable-media attempt (event `4663`)

| Step | Query intent |
|:---:|:---|
| 1 | Identify the account, host and object path |
| 2 | Confirm the policy denial was applied |
| 3 | Determine whether the same account attempted access on other hosts |
| 4 | Check for a preceding archive or staging operation on the host |

**Escalate** if staging behaviour precedes the attempt: this indicates intent rather than incidental device use.

### 4.3 Unauthorised execution (event `8004`)

| Step | Query intent |
|:---:|:---|
| 1 | Identify the file path, hash where available, and initiating process |
| 2 | Confirm AppLocker denied execution |
| 3 | Retrieve the Wazuh alert for the same host and window |
| 4 | Establish the file's origin — download, removable media, or network share |
| 5 | Check whether the same file was attempted on other endpoints |

**Escalate to critical** if the file originated from removable media or an external source, or if attempts appear on multiple hosts.

### 4.4 Privileged-group modification (event `4728`)

| Step | Query intent |
|:---:|:---|
| 1 | Identify the group, the added member and the initiating account |
| 2 | Retrieve the corresponding directory audit alert |
| 3 | Verify the change against authorised change records |
| 4 | Review other activity by the initiating account in the preceding window |
| 5 | Determine whether the added account has since authenticated |

**Treat as critical by default.** Downgrade only when the change is matched to an authorised record. Absence of a record is not evidence of legitimacy.

### 4.5 Reconnaissance (`1000001`–`1000004`)

| Step | Query intent |
|:---:|:---|
| 1 | Record source, scan type and address range covered |
| 2 | Determine whether the source is internal or external to the zone |
| 3 | Check for any subsequent connection attempt to a discovered service |
| 4 | Check for authentication activity from the same source |

**Escalate** when reconnaissance is followed by targeted connection or authentication attempts — the sequence, not the scan, is the indicator.

---

## 5. Containment

### 5.1 Automated

| Trigger | Action | Constraint |
|:---|:---|:---|
| High-confidence network signature | Source address blocked at the firewall | Restricted to signatures whose false-positive behaviour has been observed (`DE-05`) |
| Host detection meeting response criteria | Wazuh active response executes the configured action | Scoped to the affected host |
| Policy violation | Execution or device access denied by Group Policy | Applies continuously, not on alert |

### 5.2 Manual

| Ref | Action | Authorisation |
|:---|:---|:---|
| `CN-01` | Isolate the endpoint at the firewall | Analyst, on confirmed critical finding |
| `CN-02` | Disable the affected domain account | Analyst, on confirmed credential compromise |
| `CN-03` | Revert the unauthorised group membership | Analyst, after recording the pre-change state |
| `CN-04` | Widen the block to a source range | Requires explicit justification — broad blocking risks self-inflicted outage |

### 5.3 Containment constraint

Automated blocking is address-based. It **shall not** be relied upon against a rotating source, and it **shall not** be extended to low-fidelity detections: doing so converts the defence into a denial-of-service mechanism against legitimate users.

---

## 6. Evidence Handling

| Ref | Requirement |
|:---|:---|
| `EH-01` | The pre-change state **shall** be recorded before any containment action modifies it. |
| `EH-02` | Alert identifiers, timestamps and query parameters **shall** be recorded so the finding is reproducible. |
| `EH-03` | Raw exports containing user content **shall not** be published. |
| `EH-04` | Case notes **shall** record the disposition and its reasoning, including cases closed as benign. |

---

## 7. Post-Incident Activity

| Step | Activity |
|:---:|:---|
| 1 | Record the timeline from first indicator to containment |
| 2 | Identify which layer detected, and which layer should have detected earlier |
| 3 | Determine whether the detection logic requires tuning |
| 4 | Update the signature register or the correlation playbook accordingly |
| 5 | Add a regression scenario so the same gap is re-tested in future runs |

Step 5 closes the loop: every finding becomes a permanent test case rather than a one-time fix.

---

## 8. Known Runbook Limitations

This runbook has been exercised during the validation run only. It has not been tested under concurrent multi-incident load, and no on-call rotation or escalation timetable is defined, as neither applies to a laboratory operated by a project team. Both are recorded in the limitations register.

---

[← Control framework mapping](05-control-framework-mapping.md) · [Limitations and roadmap →](07-limitations-and-roadmap.md)
