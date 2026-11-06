# Document Control

| Field | Value |
|:---|:---|
| **Register identifier** | `LAYERS-SOC-000` |
| **Version** | 2.0 |
| **Status** | Released |
| **Date of issue** | 2026-11-06 |
| **Classification** | Public — laboratory environment only |

---

## 1. Purpose

This register defines the documentation set for the SOC Lab — Defense in Depth project, records the revision history of each document, and states the conventions applied throughout the set.

---

## 2. Document Register

| Identifier | File | Title | Version | Status |
|:---|:---|:---|:---:|:---|
| `LAYERS-SOC-000` | `00-document-control.md` | Document Control | 2.0 | Released |
| `LAYERS-SOC-001` | `../README.md` | Overview, Findings and Summary Results (Arabic) | 2.0 | Released |
| `LAYERS-SOC-002` | `01-architecture.md` | Network and Host Architecture | 2.0 | Released |
| `LAYERS-SOC-003` | `02-defense-layers.md` | Layered Control Design | 2.0 | Released |
| `LAYERS-SOC-004` | `03-detection-engineering.md` | Detection Engineering and Log Pipeline | 2.0 | Released |
| `LAYERS-SOC-005` | `04-validation-and-results.md` | Validation Plan and Results | 2.0 | Released |
| `LAYERS-SOC-006` | `05-control-framework-mapping.md` | Control Framework Cross-Reference | 2.0 | Released |
| `LAYERS-SOC-007` | `06-operations-runbook.md` | Triage and Response Runbook | 2.0 | Released |
| `LAYERS-SOC-008` | `07-limitations-and-roadmap.md` | Limitations and Roadmap | 2.0 | Released |
| `LAYERS-SOC-009` | `08-glossary.md` | Terms and Abbreviations | 2.0 | Released |
| `LAYERS-SOC-010` | `09-references.md` | References | 2.0 | Released |
| `LAYERS-SOC-011` | `../rules/README.md` | Custom Signature Register | 2.0 | Released |

---

## 3. Revision History

| Version | Date | Description | Status |
|:---:|:---|:---|:---|
| 1.0 | 2026-11-06 | Initial bilingual summary; repository established. | Superseded |
| 2.0 | 2026-11-06 | Full restructure into a numbered documentation set: technical documents in English, repository front matter in Arabic. Added architecture specification, per-layer control design, detection engineering specification, validation plan with ATT&CK mapping, control-framework cross-reference, operations runbook, limitations register, glossary and references. Removed references to artefacts not present in the repository. | Released |

---

## 4. Conventions

### 4.0 Language policy

| Artefact class | Language | Rationale |
|:---|:---|:---|
| Repository front matter — `README.md`, `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md` | Arabic, with technical terms retained in English | Primary readership |
| Technical documentation — `docs/01`–`docs/09`, `rules/README.md` | English | International specification phrasing; citable and reviewable by any party |
| This register — `docs/00-document-control.md` | English | Part of the technical documentation set |
| Citation metadata — `CITATION.cff` | English | Machine-readable standard format |
| Commit messages | English | Tooling convention |

Requirement language maps between the two languages as follows: `shall` → يجب, `should` → يُستحسن, `may` → يجوز, `shall not` → لا يجوز.

### 4.1 Requirement language

The key words **shall**, **should** and **may** are used in the sense of RFC 2119:

| Term | Meaning |
|:---|:---|
| **shall** | Mandatory requirement. Non-conformance invalidates the stated control objective. |
| **should** | Recommended practice. Deviation is permitted where justified and recorded. |
| **may** | Permitted option, carrying no obligation. |

### 4.2 Numbering

Sections are numbered hierarchically (`§4.2.1`). Figures and tables are numbered sequentially within each document and are always accompanied by a caption.

### 4.3 Evidence convention

A statement is presented as **verified** only where it is supported by an artefact present in this repository or by a recorded observation from the validation run. Items that are designed but not yet published are listed in `07-limitations-and-roadmap.md` and are never presented as available evidence.

### 4.4 Addressing and naming

All addresses are RFC 1918 private ranges. The domain `corp.test` uses the reserved `.test` top-level domain and does not resolve on the public Internet. Host names are laboratory identifiers.

### 4.5 Redaction policy

The following shall not appear in any published artefact:

- Credentials, keys, tokens or certificate private material
- Public IP addresses or publicly resolvable hostnames
- Personal data of any individual
- Client or employer identifying information
- Full packet captures or raw event exports containing user content

---

## 5. Review and Maintenance

| Activity | Trigger | Responsibility |
|:---|:---|:---|
| Documentation review | Any change to architecture or detection logic | Project maintainers |
| Validation re-run | Addition or modification of a control layer | Project maintainers |
| Register update | Addition, retirement or renumbering of a document | Project maintainers |
| Limitations review | Closure of any roadmap item | Project maintainers |

---

[← Back to repository overview](../README.md)
