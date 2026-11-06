# References

| Field | Value |
|:---|:---|
| **Document identifier** | `LAYERS-SOC-010` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Normative References

The following documents define the conventions and control objectives applied in this documentation set.

| Ref | Title | Source |
|:---|:---|:---|
| `[N1]` | RFC 2119 — Key words for use in RFCs to Indicate Requirement Levels | https://www.rfc-editor.org/rfc/rfc2119 |
| `[N2]` | RFC 1918 — Address Allocation for Private Internets | https://www.rfc-editor.org/rfc/rfc1918 |
| `[N3]` | RFC 2606 — Reserved Top Level DNS Names | https://www.rfc-editor.org/rfc/rfc2606 |
| `[N4]` | ISO/IEC 27001:2022 — Information security management systems — Requirements | https://www.iso.org/standard/27001 |
| `[N5]` | ISO/IEC 27002:2022 — Information security controls | https://www.iso.org/standard/75652.html |
| `[N6]` | PCI DSS v4.0 — Payment Card Industry Data Security Standard | https://www.pcisecuritystandards.org/document_library/ |
| `[N7]` | CIS Critical Security Controls v8.1 | https://www.cisecurity.org/controls/v8 |
| `[N8]` | MITRE ATT&CK Enterprise Matrix | https://attack.mitre.org/matrices/enterprise/ |
| `[N9]` | NIST SP 800-61 Rev. 2 — Computer Security Incident Handling Guide | https://csrc.nist.gov/pubs/sp/800/61/r2/final |

---

## 2. MITRE ATT&CK Techniques Referenced

| Technique | Title | Reference |
|:---|:---|:---|
| `T1046` | Network Service Discovery | https://attack.mitre.org/techniques/T1046/ |
| `T1021.002` | Remote Services: SMB/Windows Admin Shares | https://attack.mitre.org/techniques/T1021/002/ |
| `T1040` | Network Sniffing | https://attack.mitre.org/techniques/T1040/ |
| `T1048` | Exfiltration Over Alternative Protocol | https://attack.mitre.org/techniques/T1048/ |
| `T1052.001` | Exfiltration Over Physical Medium: Exfiltration over USB | https://attack.mitre.org/techniques/T1052/001/ |
| `T1071.004` | Application Layer Protocol: DNS | https://attack.mitre.org/techniques/T1071/004/ |
| `T1098` | Account Manipulation | https://attack.mitre.org/techniques/T1098/ |
| `T1110.001` | Brute Force: Password Guessing | https://attack.mitre.org/techniques/T1110/001/ |
| `T1204.002` | User Execution: Malicious File | https://attack.mitre.org/techniques/T1204/002/ |
| `T1562.001` | Impair Defenses: Disable or Modify Tools | https://attack.mitre.org/techniques/T1562/001/ |

Technique identifiers and titles were verified against the published ATT&CK knowledge base at the date of issue. ATT&CK is a living knowledge base; identifiers are stable but descriptions and sub-technique sets are revised between releases.

---

## 3. Platform and Product Documentation

| Ref | Component | Documentation |
|:---|:---|:---|
| `[P1]` | pfSense | https://docs.netgate.com/pfsense/en/latest/ |
| `[P2]` | Snort | https://www.snort.org/documents |
| `[P3]` | Splunk Enterprise | https://docs.splunk.com/Documentation/Splunk |
| `[P4]` | Splunk Universal Forwarder | https://docs.splunk.com/Documentation/Forwarder |
| `[P5]` | Wazuh | https://documentation.wazuh.com/ |
| `[P6]` | Microsoft Sysmon | https://learn.microsoft.com/sysinternals/downloads/sysmon |
| `[P7]` | Microsoft AppLocker | https://learn.microsoft.com/windows/security/application-security/application-control/app-control-for-business/applocker/applocker-overview |
| `[P8]` | Windows Security audit events | https://learn.microsoft.com/windows/security/threat-protection/auditing/security-auditing-overview |
| `[P9]` | Active Directory Domain Services | https://learn.microsoft.com/windows-server/identity/ad-ds/active-directory-domain-services |
| `[P10]` | Kali Linux | https://www.kali.org/docs/ |

---

## 4. Windows Event Identifiers Referenced

| Event ID | Title | Channel |
|:---:|:---|:---|
| `4625` | An account failed to log on | Security |
| `4663` | An attempt was made to access an object | Security |
| `4728` | A member was added to a security-enabled global group | Security |
| `8004` | A file was prevented from running | AppLocker (MSI and Script / EXE and DLL) |

---

## 5. Informative References

The following secondary sources were consulted to confirm control identifiers and titles for the cross-reference in `05-control-framework-mapping.md`. They are informative only; the authoritative definitions remain the standards listed in §1.

| Ref | Description | Source |
|:---|:---|:---|
| `[I1]` | ISO/IEC 27001:2022 Annex A control list (93 controls, four themes) | https://iseoblue.com/iso-27001/annex-a/iso-27001-controls-list/ |
| `[I2]` | Annex A control descriptions | https://consultantslikeus.co.uk/post/iso27001-2022-annex-a-controls/ |
| `[I3]` | CIS Controls v8.1 structure and safeguard register | https://www.cisecurity.org/controls/v8 |
| `[I4]` | MITRE ATT&CK technique mappings explorer | https://ctid.mitre.org/mappings/ |
| `[I5]` | MITRE D3FEND defensive technique taxonomy | https://d3fend.mitre.org/ |

---

## 6. Third-Party Ruleset Licensing

The laboratory subscribes to public Snort rulesets. Their content is **not redistributed** in this repository. Each retains its own licence and terms:

| Ruleset | Terms |
|:---|:---|
| Cisco Talos VRT | Talos ruleset licence — registration required; redistribution restricted |
| Snort Community Rules | GPLv2 |
| Emerging Threats Open | BSD-style open licence |
| OpenAppID | Published by Cisco for application identification |

Custom signatures in the private SID range are original work of the project team and are released under the repository's MIT Licence.

---

## 7. Citation

To reference this work, use the metadata in [`../CITATION.cff`](../CITATION.cff).

---

[← Glossary](08-glossary.md) · [Back to overview →](../README.md)
