# Terms and Abbreviations

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-009` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Terms

| Term | Definition as used in this documentation set |
|:---|:---|
| **Adversary emulation** | Authorised execution of behaviour representative of a documented technique, for the purpose of measuring defensive response. |
| **Artefact** | An observable output of a control — an alert, an event record or a policy denial — used as evidence in validation. |
| **Control objective** | The security outcome a layer is intended to achieve, stated independently of the product used to achieve it. |
| **Coverage** | The existence of detection logic for a technique. Coverage does not imply validation. |
| **Defence in depth** | A design approach in which each layer assumes the preceding layer may fail, so that no single control carries an objective alone. |
| **Detection engineering** | The disciplined authoring, thresholding, testing and maintenance of detection logic. |
| **East–west traffic** | Traffic between hosts within the same segment. It does not traverse the perimeter and is therefore invisible to perimeter inspection. |
| **Expected artefact** | The specific observable output declared before a test case is executed. A case passes only if this artefact is observed. |
| **False assurance** | The state in which a control appears to be operating while its telemetry has silently stopped. More dangerous than a visible failure. |
| **Prevented** | The activity was denied by a control. Distinct from *detected*, and the two are recorded separately. |
| **Residual risk** | The risk remaining after a layer has been applied. Stated explicitly for every layer. |
| **Signature** | A network detection rule expressed as a pattern with an associated action, message and severity. |
| **Trust zone** | A network segment whose members share a common trust assumption. Crossing a zone boundary requires traversal of the firewall. |
| **Validated** | Detection logic that was exercised in a test case and produced its declared artefact. |

---

## 2. Abbreviations

| Abbreviation | Expansion |
|:---|:---|
| **AD DS** | Active Directory Domain Services |
| **ATT&CK** | Adversarial Tactics, Techniques, and Common Knowledge |
| **C2** | Command and Control |
| **CIS** | Center for Internet Security |
| **DNS** | Domain Name System |
| **FTP** | File Transfer Protocol |
| **GPO** | Group Policy Object |
| **HIDS** | Host-based Intrusion Detection System |
| **ICMP** | Internet Control Message Protocol |
| **IDS** | Intrusion Detection System |
| **IPS** | Intrusion Prevention System |
| **ISO/IEC** | International Organization for Standardization / International Electrotechnical Commission |
| **MITRE** | The MITRE Corporation |
| **NAT** | Network Address Translation |
| **NIST** | National Institute of Standards and Technology |
| **OU** | Organizational Unit |
| **PCI DSS** | Payment Card Industry Data Security Standard |
| **RDP** | Remote Desktop Protocol |
| **RFC** | Request for Comments |
| **SID** | Signature Identifier (Snort) |
| **SIEM** | Security Information and Event Management |
| **SMB** | Server Message Block |
| **SOC** | Security Operations Centre |
| **SP** | Special Publication (NIST) |
| **SSH** | Secure Shell |
| **SYN** | Synchronise (TCP flag) |
| **TLS** | Transport Layer Security |
| **UDP** | User Datagram Protocol |
| **USB** | Universal Serial Bus |
| **VM** | Virtual Machine |
| **VRT** | Vulnerability Research Team (Cisco Talos) |

---

## 3. Identifier Conventions

| Prefix | Meaning | Example |
|:---|:---|:---|
| `AR-nn` | Architecture requirement | `AR-03` |
| `DE-nn` | Detection engineering rule | `DE-02` |
| `LT-nn` | Log transport requirement | `LT-01` |
| `AC-nn` | Acceptance criterion | `AC-04` |
| `TC-nn` | Test case | `TC-02` |
| `F-nn` | Finding | `F-01` |
| `L-nn` | Limitation | `L-09` |
| `G-nn` | Framework gap | `G-01` |
| `RR-nn` | Residual architectural risk | `RR-02` |
| `CN-nn` | Containment action | `CN-03` |
| `EH-nn` | Evidence handling requirement | `EH-01` |
| `DG-nn` | Detection gap | `DG-01` |
| `H-nn` | Host reference | `H-03` |
| `LAYERS-SOC-nnn` | Document identifier | `LAYERS-SOC-005` |

---

## 4. Laboratory Identifiers

| Identifier | Meaning |
|:---|:---|
| `corp.test` | Reserved laboratory directory domain; does not resolve publicly (RFC 2606) |
| `LAN` | Corporate trust zone, `10.10.10.0/24` |
| `UNTRUST` | Adversary trust zone, `10.10.40.0/24` |
| `WAN` | Simulated external transit, NAT-assigned |

All addresses are RFC 1918 private ranges and are not routable on the public Internet.

---

[← Limitations and roadmap](07-limitations-and-roadmap.md) · [References →](09-references.md)
