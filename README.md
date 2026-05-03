# 🛡️ APT29 CTI Pipeline — LLM-Based Threat Intelligence & STIX 2.1 Generation

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10-3572A5?style=for-the-badge&logo=python&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)
![OpenAI](https://img.shields.io/badge/GPT--4o-412991?style=for-the-badge&logo=openai&logoColor=white)
![STIX](https://img.shields.io/badge/STIX_2.1-E34234?style=for-the-badge&logoColor=white)
![MITRE](https://img.shields.io/badge/MITRE_ATT%26CK-003865?style=for-the-badge&logoColor=white)

**Student:** Sarmad Farooq &nbsp;|&nbsp; **Roll No:** 25i-7722 &nbsp;|&nbsp; **Activity:** 4 &nbsp;|&nbsp; **Instructor:** Dr. Zafar &nbsp;|&nbsp; **Institute:** FAST-NUCES

</div>

---

## 📌 Project Overview

This project implements a fully automated **Cyber Threat Intelligence (CTI) extraction and structuring pipeline** built in Google Colab. It processes the APT29 *"Cozy Bear"* CTI Report (Operation PHANTOM BEAR) through a multi-stage pipeline:

- Ingests a raw PDF threat intelligence report
- Extracts and cleans full text using **pdfplumber**
- Splits text into LLM-safe chunks and sends to **GPT-4o** for entity extraction
- Maps extracted entities to **STIX 2.1 SDOs and SROs** using the `stix2` Python library
- Assembles a validated **STIX 2.1 Bundle** and exports as JSON
- Visualizes the threat knowledge graph using the OASIS STIX Visualizer

The pipeline produces a **66-object STIX 2.1 knowledge graph** covering the full threat landscape of APT29's Operation PHANTOM BEAR campaign targeting European diplomatic missions in 2023–2024.

---

## 📊 Output Statistics

| Metric | Value |
|--------|-------|
| 🧩 Total STIX Objects | **66** |
| 🔗 Relationship SROs | **34** |
| 🎯 MITRE ATT&CK TTPs | **10** |
| 🚨 IOC Indicators | **9** |
| 🦠 Malware Families | **3** |
| 🔓 Vulnerabilities (CVEs) | **3** |
| 🏗️ Infrastructure Objects | **2** |
| 📓 Notebook Cells | **27** |

---

## 🔁 Pipeline Architecture

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐    ┌──────────────┐
│  PDF Upload  │───▶│ Text Extract │───▶│ Clean+Chunk │───▶│  GPT-4o NER  │
│ (Colab GUI) │    │ (pdfplumber) │    │  (12k chars)│    │ (Structured) │
└─────────────┘    └──────────────┘    └─────────────┘    └──────┬───────┘
                                                                   │
┌─────────────┐    ┌──────────────┐    ┌─────────────┐    ┌──────▼───────┐
│  Graph Viz  │◀───│   Validate   │◀───│ STIX Bundle │◀───│ STIX Mapping │
│(OASIS Tool) │    │ (Compliance) │    │  (Export)   │    │ SDOs + SROs  │
└─────────────┘    └──────────────┘    └─────────────┘    └──────────────┘
```

---

## 📁 Repository Structure

```
APT29-CTI-STIX21-Pipeline/
│
├── 📓 APT29_CTI_STIX21_Pipeline.ipynb   # Main Google Colab notebook (27 cells)
├── 📦 APT29_STIX21_Bundle.json          # Exported STIX 2.1 bundle (66 objects)
├── 📄 APT29_CTI_Report.pdf              # Source CTI report — TLP:WHITE
├── 🖼️  Activity4_Presentation.pptx       # 18-slide pipeline walkthrough
├── 🌐 APT29_CTI_Portfolio.html          # Interactive GitHub portfolio page
└── 📝 README.md                         # This file
```

---

## 🧠 Threat Actor: APT29 / Cozy Bear

| Attribute | Value |
|-----------|-------|
| **Primary Name** | APT29 / Cozy Bear |
| **Aliases** | Midnight Blizzard · NOBELIUM · The Dukes · Dark Halo · Iron Hemlock |
| **Sponsoring Nation** | Russian Federation |
| **Sponsoring Agency** | SVR (Foreign Intelligence Service) / FSB |
| **Active Since** | ~2008 |
| **Motivation** | Espionage · Intelligence Collection · Political Influence |
| **Sophistication** | Expert |
| **Primary Targets** | Government · Diplomatic · Defense · Healthcare · Tech |
| **Confidence** | High (85%) |
| **Report ID** | CTI-2024-APT29-0042 |

---

## 🗂️ STIX 2.1 Object Breakdown

| STIX Type | Count | Description |
|-----------|-------|-------------|
| `threat-actor` | 1 | APT29 / Cozy Bear |
| `campaign` | 1 | Operation PHANTOM BEAR |
| `malware` | 3 | WINELOADER · TEARDROP · SUNBURST |
| `attack-pattern` | 10 | MITRE ATT&CK techniques |
| `indicator` | 9 | IPs · Domains · Hashes · URLs |
| `infrastructure` | 2 | C2 + Phishing infrastructure |
| `vulnerability` | 3 | CVE-2023-23397 · CVE-2020-10148 · CVE-2019-19781 |
| `course-of-action` | 1 | Defensive mitigations |
| `relationship` | 34 | SROs linking all objects |
| `report` | 1 | Wrapping report SDO |
| `identity` | 1 | Threat Intelligence Unit |
| **Total** | **66** | |

---

## 🦠 Malware Families Analyzed

### WINELOADER
> Newly identified modular backdoor attributed exclusively to APT29

- **Type:** Backdoor / Dropper
- **Delivery:** ZIP archive via spear-phishing email
- **Technique:** Process hollowing into `sqlwriter.exe`
- **C2 Protocol:** HTTPS port 443 with RC4 + Base64 encryption
- **Persistence:** `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
- **MD5:** `a3f5c2b1d4e6789012345678abcdef01`
- **SHA-256:** `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`

### TEARDROP
> Memory-only second-stage dropper — leaves no disk artifacts

- **Type:** Memory-only Dropper
- **Technique:** Reads fake `favicon.jpg` to extract embedded payload
- **Payload:** Custom Cobalt Strike beacon (malleable C2)
- **Detection:** Challenging — no disk writes, no forensic artifacts
- **SHA-256:** `7acf71afa895df5358b0ede2d71128634bfbbc0e2d9deccff5c5eaa25e6f5510`

### SUNBURST (Historical Context)
> 2020 SolarWinds supply chain implant — infrastructure overlap identified

- **CVE:** CVE-2020-10148 (CVSS 10.0)
- **Infrastructure overlaps** with Operation PHANTOM BEAR detected

---

## 📅 Campaign Timeline: Operation PHANTOM BEAR

```
Oct 2023 ──▶ Initial spear-phishing emails sent to European embassies
             (wine-tasting event invitations as social engineering lure)

Nov 2023 ──▶ WINELOADER dropper deployed on ambassador workstations
             Process hollowing into sqlwriter.exe; Registry Run persistence

Dec 2023 ──▶ Lateral movement observed
             Credential harvesting via Mimikatz / LSASS memory dump

Jan 2024 ──▶ TEARDROP second-stage delivered
             Custom Cobalt Strike beacon loaded; data exfiltration begins

Feb 2024 ──▶ Campaign detected by defenders
             C2 infrastructure rotated to new IP ranges
```

---

## 🚨 Indicators of Compromise (IOCs)

### Network Indicators

| Type | Value | Context |
|------|-------|---------|
| IP | `185.220.101.45` | WINELOADER C2 — AS59796 Serverius |
| IP | `91.108.4.200` | TEARDROP C2 — AS20473 Vultr |
| IP | `194.165.16.78` | Phishing Infrastructure |
| Domain | `update.microsoftonline-cdn.com` | Fake Microsoft C2 domain |
| Domain | `cdn-telemetry.azure-api.net` | Typosquat — TEARDROP payload URL |
| Domain | `oauth2.live-signin.com` | Credential phishing site |
| URL | `https://update.microsoftonline-cdn.com/api/v2/push` | C2 endpoint |

### File Indicators

| Type | Hash | File |
|------|------|------|
| SHA-256 | `e3b0c44298fc1c149afbf4c8996fb924...` | `wineloader.dll` |
| SHA-256 | `7acf71afa895df5358b0ede2d7112863...` | `teardrop.exe` |
| SHA-256 | `aabbcc1234567890aabbcc1234567890...` | `invitation.zip` |
| MD5 | `a3f5c2b1d4e6789012345678abcdef01` | `wineloader.dll` |
| MD5 | `5f4dcc3b5aa765d61d8327deb882cf99` | `favicon.jpg` |

---

## 🎯 MITRE ATT&CK TTP Coverage

| ATT&CK ID | Technique | Tactic |
|-----------|-----------|--------|
| T1566.001 | Spearphishing Attachment | Initial Access |
| T1055.012 | Process Hollowing | Defense Evasion |
| T1547.001 | Registry Run Keys / Startup Folder | Persistence |
| T1071.001 | Web Protocols (HTTPS C2) | Command & Control |
| T1573.001 | Symmetric Cryptography (RC4) | Command & Control |
| T1003.001 | LSASS Memory Credential Dump | Credential Access |
| T1027 | Obfuscated Files (Base64) | Defense Evasion |
| T1041 | Exfiltration Over C2 Channel | Exfiltration |
| T1078 | Valid Accounts (stolen credentials) | Lateral Movement |
| T1195.002 | Compromise Software Supply Chain | Initial Access |

---

## 🤖 LLM Prompt Engineering

The pipeline uses a **two-layer prompt design** for GPT-4o:

```
SYSTEM PROMPT:
You are an expert Cyber Threat Intelligence (CTI) analyst.
Extract structured entities from the threat intelligence text.

Entity types to extract:
  - threat_actors:   name, aliases, description, motivation
  - malware:         name, type, description, capabilities
  - indicators:      type [ip/domain/hash/url], value, context
  - attack_patterns: name, mitre_id, description, phase
  - vulnerabilities: cve_id, description, cvss, product
  - infrastructure:  name, type, description
  - relationships:   source, target, relationship_type

Return ONLY a valid JSON object. No preamble, no markdown.
```

Text chunks of **12,000 characters** are processed sequentially, and results are **merged + de-duplicated** before STIX mapping.

---

## 🔧 Tech Stack

| Component | Tool / Library |
|-----------|---------------|
| Environment | Google Colab |
| Language | Python 3.10 |
| PDF Extraction | `pdfplumber` |
| LLM API | OpenAI GPT-4o |
| STIX 2.1 | `stix2` Python library |
| Data Processing | `pandas`, `json`, `re` |
| Visualization | OASIS STIX Visualizer |
| Presentation | PowerPoint (18 slides) |
| Portfolio | HTML / CSS / JS |

---

## 🚀 Setup & Usage

### Prerequisites

```bash
pip install pdfplumber openai stix2 requests ipywidgets pandas
```

### Run in Google Colab (Recommended)

1. Open `APT29_CTI_STIX21_Pipeline.ipynb` in **Google Colab**
2. Add your `OPENAI_API_KEY` to **Colab Secrets** (🔑 key icon in sidebar)
3. Run all cells: **Runtime → Run All**
4. Upload `APT29_CTI_Report.pdf` when the file widget appears
5. Download `APT29_STIX21_Bundle.json` after the final cell completes

### Visualize the Knowledge Graph

Load `APT29_STIX21_Bundle.json` into the **OASIS STIX Visualizer**:
[https://oasis-open.github.io/cti-stix-visualization/](https://oasis-open.github.io/cti-stix-visualization/)

---

## ✅ STIX 2.1 Validation

The bundle was validated against STIX 2.1 compliance checks:

- ✅ All objects contain required fields (`type`, `id`, `spec_version`, `created`, `modified`)
- ✅ All `id` fields follow the `type--UUID4` format
- ✅ All `relationship_type` values use valid STIX verbs (`uses`, `attributed-to`, `indicates`, `targets`, `mitigates`, `exploits`, `delivers`, `drops`)
- ✅ All timestamps in ISO 8601 UTC format
- ✅ Bundle wraps all 65 objects under a single `report` SDO

---

## 🛡️ Defensive Recommendations

Based on the extracted threat intelligence:

1. **Block** known C2 domains and IP ranges at perimeter firewall and DNS sinkholes
2. **Patch** CVE-2023-23397 immediately — Microsoft Outlook zero-click (CVSS 9.8)
3. **Enable** MFA for all remote access and email services
4. **Deploy** EDR with memory scanning to detect process hollowing
5. **Monitor** for `sqlwriter.exe` and `svchost.exe` anomalous behavior
6. **Implement** network segmentation to limit lateral movement
7. **Train** users on spear-phishing recognition (wine-tasting lure pattern)
8. **Hunt** for registry key: `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`

---

## 👤 Author

| Field | Detail |
|-------|--------|
| **Name** | Sarmad Farooq |
| **Roll No** | 25i-7722 |
| **Activity** | Activity 4 — CTI Pipeline |
| **Instructor** | Dr. Zafar |
| **Institute** | FAST-NUCES, Islamabad |
| **Course** | Cyber Security Lab |

---

## 📋 Report Metadata

| Field | Value |
|-------|-------|
| Report ID | CTI-2024-APT29-0042 |
| Date | March 2024 |
| Author | Threat Intelligence Unit |
| Classification | TLP:WHITE — Unrestricted Distribution |
| Confidence | High (85%) |

---

<div align="center">

*Built with Python · GPT-4o · STIX 2.1 · Google Colab*

**FAST-NUCES Islamabad — Cyber Security Lab — Activity 4**

</div>
