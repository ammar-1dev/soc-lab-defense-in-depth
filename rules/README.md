# Custom Signature Register

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-011` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **SID range** | `1000001` – `1000014` (private range) |
| **Classification** | Public — laboratory environment only |

---

## 1. Purpose

This register is the authoritative record of the fourteen custom network detection signatures authored for the laboratory. It documents each signature's identifier, detection intent, severity, mapped technique and validation status.

---

## 2. Publication Status

| Artefact | Status |
|:---|:---|
| Signature register (this document) | **Published** |
| Raw signature file (`local.rules`) | **Pending** — awaiting address sanitisation and final review |

The raw file is withheld until every address field references `$HOME_NET` or `$EXTERNAL_NET` rather than a literal address, in accordance with authoring rule `DE-02`. Publishing an unsanitised rule file would disclose internal addressing and would also make the rules non-portable.

No signature syntax is reproduced in this document. Descriptions state intent, not implementation, so that nothing here can be mistaken for a tested, deployable rule.

---

## 3. Register

| SID | Detection intent | Severity | ATT&CK | Validated |
|:---:|:---|:---:|:---|:---:|
| `1000001` | ICMP echo reconnaissance from the adversary zone | Medium | [T1046](https://attack.mitre.org/techniques/T1046/) | — |
| `1000002` | TCP SYN service discovery | Medium | [T1046](https://attack.mitre.org/techniques/T1046/) | ✔ TC-01 |
| `1000003` | TCP XMAS flag-pattern scan | Medium | [T1046](https://attack.mitre.org/techniques/T1046/) | — |
| `1000004` | TCP NULL flag-pattern scan | Medium | [T1046](https://attack.mitre.org/techniques/T1046/) | — |
| `1000005` | RDP credential guessing | High | [T1110.001](https://attack.mitre.org/techniques/T1110/001/) | ✔ TC-02 |
| `1000006` | SMB access attempt from the adversary zone | High | [T1021.002](https://attack.mitre.org/techniques/T1021/002/) | — |
| `1000007` | SSH credential guessing | High | [T1110.001](https://attack.mitre.org/techniques/T1110/001/) | — |
| `1000008` | Anti-malware test artefact in transit | Low | Control validation | — |
| `1000009` | DNS behaviour consistent with command-and-control | High | [T1071.004](https://attack.mitre.org/techniques/T1071/004/) | — |
| `1000010` | Outbound transfer consistent with exfiltration | Critical | [T1048](https://attack.mitre.org/techniques/T1048/) | — |
| `1000011` | SIEM interface access from a non-management source | High | [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | — |
| `1000012` | HIDS interface access from a non-management source | High | [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | — |
| `1000013` | Telnet session establishment — cleartext protocol | Medium | [T1040](https://attack.mitre.org/techniques/T1040/) | — |
| `1000014` | FTP session establishment — cleartext protocol | Medium | [T1040](https://attack.mitre.org/techniques/T1040/) | — |

**Validated:** 2 of 14 signatures were exercised directly in the validation run. The remaining twelve have deployed detection logic but were not exercised, and are therefore recorded as coverage rather than as validated detection.

> Test cases TC-03, TC-04 and TC-05 passed on host-based and policy evidence rather than on network signatures. See [`../docs/04-validation-and-results.md`](../docs/04-validation-and-results.md).

---

## 4. Severity Assignment

| Severity | Assignment basis |
|:---:|:---|
| **Critical** | Potential loss of data from the trust zone |
| **High** | Credential access, lateral movement, or tampering with security tooling |
| **Medium** | Reconnaissance or insecure protocol use, with no immediate compromise |
| **Low** | Control-validation artefact with no adversary significance |

Severity reflects impact and rarity, not the volume of alerts produced.

---

## 5. Authoring Rules

| Ref | Rule |
|:---|:---|
| `DE-01` | Every custom signature **shall** use a SID within `1000001`–`1999999`. |
| `DE-02` | Address fields **shall** reference `$HOME_NET` and `$EXTERNAL_NET`, never literal addresses. |
| `DE-03` | Every signature **shall** carry a message, a reference and a severity classification. |
| `DE-04` | Rate limiting **should** be applied where normal administrative activity can trigger the signature. |
| `DE-05` | A signature **shall not** be enabled for automated blocking before its false-positive behaviour has been observed against baseline traffic. |
| `DE-06` | Every signature **shall** declare an expected artefact before being exercised in validation. |

### 5.1 Note on `DE-05`

Automated blocking on an untuned signature is a self-inflicted denial-of-service risk. In this laboratory, automated blocking is restricted to signatures whose behaviour has been observed against baseline traffic. Signatures `1000011` and `1000012` in particular can be triggered by legitimate administrative access from an unexpected source and are therefore excluded from automated blocking.

---

## 6. Register Maintenance

| Change | Required action |
|:---|:---|
| New signature | Allocate the next free SID; add a register row; declare an expected artefact |
| Modified logic | Increment the rule revision; re-run the associated test case |
| Retired signature | Mark as retired in the register; never reuse the SID |
| Validation performed | Update the *Validated* column and the corresponding test case reference |

A SID **shall not** be reused after retirement: historical alerts must remain interpretable.

---

## 7. Third-Party Rulesets

Subscribed public rulesets operate alongside the custom set and retain their own licences. Their content is not redistributed here. See [`../docs/09-references.md`](../docs/09-references.md), §6.

---

[← Detection engineering](../docs/03-detection-engineering.md) · [Back to overview →](../README.md)
