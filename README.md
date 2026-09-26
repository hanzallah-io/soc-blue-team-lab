<h1 align="center">🛡️ SOC Blue Team Lab — Cyberster Internship</h1>

<p align="center">
  <strong>Detect • Investigate • Respond • Report</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/STATUS-COMPLETED-brightgreen">
  <img src="https://img.shields.io/badge/DURATION-90%20DAYS%20%7C%2012%20WEEKS-blue">
  <img src="https://img.shields.io/badge/ROLE-SOC%20ANALYST%20INTERN-purple">
  <img src="https://img.shields.io/badge/WAZUH-SIEM-red">
  <img src="https://img.shields.io/badge/MITRE-ATT%26CK-orange">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Windows%2011-0078D6?logo=windows&logoColor=white">
  <img src="https://img.shields.io/badge/Kali%20Linux-557C94?logo=kalilinux&logoColor=white">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu&logoColor=white">
  <img src="https://img.shields.io/badge/pfSense-212121?logo=pfsense&logoColor=white">
  <img src="https://img.shields.io/badge/Suricata-D3212D">
</p>

---

## 📌 Project Overview

The **SOC Blue Team Lab** is a hands-on, multi-VM security operations environment developed during a **90-day SOC Analyst internship at Cyberster**.

The lab was built progressively over 12 weeks, evolving from basic endpoint monitoring and log analysis into a broader blue-team workflow covering:

**Telemetry → Detection → Investigation → Threat Intelligence → Incident Response → Digital Forensics**

Rather than following a single tutorial, the project documents the configuration, testing, troubleshooting, detection engineering, and investigations performed throughout the internship.

---

## 🎯 Objectives

* Build a segmented multi-VM SOC environment
* Deploy and configure **Wazuh SIEM** for endpoint monitoring
* Configure **File Integrity Monitoring (FIM)** and Windows/Linux telemetry
* Develop and validate **custom Wazuh and Suricata detection rules**
* Implement **network segmentation and firewall controls with pfSense**
* Enrich security events using **VirusTotal and URLhaus**
* Map detections and investigations to **MITRE ATT&CK**
* Perform incident response using **NIST SP 800-61**
* Conduct digital forensics using **Volatility 3, Autopsy, LECmd, and browser artifacts**
* Document findings, troubleshooting, evidence, and remediation

---

## 🏗️ Lab Architecture

### Environment

| Component         | Role                                 | IP               |
| ----------------- | ------------------------------------ | ---------------- |
| **Kali Linux**    | Attack simulation / security testing | `192.168.56.102` |
| **Ubuntu Server** | Wazuh Manager / SIEM                 | `192.168.56.10`  |
| **Windows 11**    | Monitored endpoint / Wazuh Agent     | `192.168.56.1`   |
| **pfSense**       | Firewall / routing / segmentation    | `192.168.56.2`   |

The lab initially used a VirtualBox host-only network and was subsequently routed through **pfSense** to introduce firewall filtering and network segmentation.

### Security Workflow

```text
                         ┌─────────────────────┐
                         │     Kali Linux      │
                         │ Attack Simulation   │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       pfSense       │
                         │ Firewall / Routing  │
                         │    Segmentation     │
                         └──────────┬──────────┘
                                    │
                     ┌──────────────┴──────────────┐
                     │                             │
                     ▼                             ▼
             ┌───────────────┐             ┌───────────────┐
             │  Windows 11   │             │ Ubuntu Server │
             │ Wazuh Agent   │────────────►│ Wazuh Manager │
             │    Endpoint   │   Telemetry │     / SIEM    │
             └───────────────┘             └───────┬───────┘
                                                   │
                         ┌─────────────────────────┼─────────────────────┐
                         │                         │                     │
                         ▼                         ▼                     ▼
                  Detection Rules          Threat Intelligence       Incident
                  Wazuh / Suricata         VirusTotal / URLhaus       Response
                                                                           │
                                                                           ▼
                                                                  Digital Forensics
```

---

## 🔄 SOC Investigation Workflow

The lab follows a simplified SOC workflow:

```text
1. Generate / Observe Event
            ↓
2. Collect Telemetry
            ↓
3. Detect & Alert
            ↓
4. Investigate Evidence
            ↓
5. Enrich with Threat Intelligence
            ↓
6. Map to MITRE ATT&CK
            ↓
7. Respond / Contain
            ↓
8. Document Findings
            ↓
9. Perform Forensic Analysis
```

---

## 📂 Repository Structure

| Directory                  | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| `01-Lab-Setup`             | VM deployment, networking, and base configuration            |
| `02-Wazuh-Deployment`      | Wazuh Manager, agent enrollment, FIM, and event monitoring   |
| `03-Detection-Engineering` | Custom Wazuh and Suricata detection rules                    |
| `04-Network-Segmentation`  | pfSense routing, firewall rules, and segmentation            |
| `05-Threat-Intelligence`   | VirusTotal, URLhaus, IOC enrichment, and MITRE mapping       |
| `06-Incident-Response`     | NIST SP 800-61 procedures and incident investigations        |
| `07-Digital-Forensics`     | Browser, LNK, USB, memory, and disk forensics                |
| `docs-archive`             | Week-by-week internship documentation and supporting reports |

---

# 🔍 Detection Engineering

Detection engineering was performed using both **Wazuh** and **Suricata**.

### Wazuh

* Windows/Linux endpoint monitoring
* File Integrity Monitoring
* Custom detection rules
* MITRE ATT&CK mapping
* SSH brute-force detection
* Active Response using `firewall-drop`
* Raw alert JSON analysis for troubleshooting

### Suricata

* IDS deployment using `af-packet`
* Custom detection rules
* Network traffic inspection
* Offline PCAP replay
* Alert validation

The detection workflow focused not only on creating rules, but also on **testing whether the expected telemetry actually reached the detection layer**.

---

# 🧪 Notable Technical Findings

## 1. VirusTotal API v2 → v3 Integration Issue

The bundled VirusTotal integration script used by the Wazuh environment targeted the deprecated **VirusTotal API v2 endpoint**.

The integration was investigated and patched to use the **v3 API endpoint** with header-based authentication.

**Result:** restored VirusTotal enrichment functionality and documented the compatibility issue as a visibility-gap finding.

---

## 2. Silent Wazuh FIM Detection Failure

A custom Wazuh File Integrity Monitoring rule initially failed to trigger despite apparently correct service configuration.

The issue was identified by inspecting the **raw Wazuh alert JSON** and comparing the actual event structure with the rule conditions.

The rule referenced:

```text
file
```

while the relevant FIM event field was:

```text
syscheck.path
```

**Result:** corrected the rule and validated the expected detection.

**Key lesson:** successful service startup does not necessarily mean that a detection rule is receiving or matching the expected telemetry.

---

## 3. pfSense / VirtualBox Virtualization Issue

The pfSense VM initially failed to boot correctly because of a virtualization compatibility issue involving **Hyper-V and VT-x**.

The issue was resolved by adjusting the VirtualBox virtualization/paravirtualization configuration and correcting the guest OS configuration.

**Result:** pfSense was successfully deployed as the lab's routing and firewall layer.

---

# 🧠 Skills Demonstrated

### SIEM & Detection Engineering

`Wazuh` `FIM` `Custom Rules` `MITRE ATT&CK` `Active Response`

* Wazuh Manager deployment
* Endpoint agent enrollment
* File Integrity Monitoring
* Custom detection rules
* Raw alert investigation
* MITRE ATT&CK mapping
* Automated firewall response

### Network Security

`pfSense` `Suricata` `Wireshark` `iptables` `ipset`

* Network segmentation
* Firewall configuration
* Routing
* IDS deployment
* ARP/DNS/HTTP/QUIC/ICMPv6 analysis
* Suricata `af-packet` configuration
* Offline PCAP analysis
* GeoIP-based filtering

### Threat Intelligence

`VirusTotal` `URLhaus` `Abuse.ch`

* IOC enrichment
* Malware/URL reputation analysis
* VirusTotal API v3 integration
* URLhaus intelligence
* IOC/IOA extraction
* Threat intelligence correlation

### Incident Response

`NIST SP 800-61` `CyberChef` `IOC Analysis`

* Incident response planning
* Evidence collection
* Insider-threat simulation
* Staging and exfiltration analysis
* Encoding/decoding
* IOC identification
* Investigation documentation

### Digital Forensics

`Volatility 3` `Autopsy` `LECmd`

* Memory forensics
* Disk-image analysis
* Browser artifact analysis
* Chrome / Edge / Firefox / Brave artifacts
* LNK file analysis
* USB device artifacts
* `.E01` forensic images

### Malware Analysis

`ANY.RUN`

* Dynamic malware analysis
* IOC extraction
* Behavioral observation
* Threat intelligence correlation

---

# 📊 Investigation Coverage

| Area                    | Technologies / Techniques             |
| ----------------------- | ------------------------------------- |
| **SIEM**                | Wazuh                                 |
| **IDS**                 | Suricata                              |
| **Firewall**            | pfSense                               |
| **Network Analysis**    | Wireshark                             |
| **Threat Intelligence** | VirusTotal, URLhaus                   |
| **Detection**           | Wazuh Rules, Suricata Rules           |
| **Frameworks**          | MITRE ATT&CK, NIST SP 800-61          |
| **Incident Response**   | Investigation, containment, reporting |
| **Memory Forensics**    | Volatility 3                          |
| **Disk Forensics**      | Autopsy, `.E01`                       |
| **Artifact Analysis**   | Browser, LNK, USB                     |
| **Malware Analysis**    | ANY.RUN                               |
| **Automation**          | Wazuh Active Response                 |

---

# 📁 Documentation

The repository contains both technical implementation notes and investigation reports.

Each major area documents the relevant:

* Configuration
* Commands
* Detection logic
* Testing methodology
* Evidence
* Findings
* Troubleshooting
* Investigation results

The `docs-archive` directory contains the complete week-by-week internship documentation.

---

# 🎓 Internship Context

This project was developed as part of the **Cyberster SOC Analyst Internship — Batch 2** over a 90-day period.

The work progressed from foundational SOC concepts to practical implementation across:

**Endpoint Monitoring → SIEM → Detection Engineering → Network Security → Threat Intelligence → Incident Response → Digital Forensics**

Weekly progress and selected internship work were also documented on LinkedIn.

🔗 **LinkedIn:** https://www.linkedin.com/in/hanzallah-io/

---

# 🚀 Key Takeaway

This repository represents a **hands-on blue-team lab built around investigation and detection workflows**, rather than a collection of isolated tool installations.

The primary objective was to understand how security telemetry moves through a SOC:

> **Collect → Detect → Investigate → Enrich → Respond → Report**

and to document the technical decisions, failures, troubleshooting, and evidence encountered while building that workflow.

---

## 👨‍💻 Author

**Muhammad Hanzallah**

BS Data Science | Cybersecurity & Blue Team Operations

GitHub: **[hanzallah-io](https://github.com/hanzallah-io)**
LinkedIn: **[hanzallah-io](https://www.linkedin.com/in/hanzallah-io/)**

---
