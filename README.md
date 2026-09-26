<h1 align="center">🛡️ SOC Blue Team Lab — Cyberster Internship</h1>

<p align="center">
🔴 Detect • 🟣 Investigate • 🟢 Respond • 🔵 Report
</p>

<p align="center">
<img src="https://img.shields.io/badge/STATUS-COMPLETED-brightgreen">
<img src="https://img.shields.io/badge/DURATION-90%20DAYS%20%7C%2012%20WEEKS-blue">
<img src="https://img.shields.io/badge/BLUE_TEAM-INTERNSHIP-purple">
<img src="https://img.shields.io/badge/WAZUH-SIEM-red">
<img src="https://img.shields.io/badge/MITRE-ATT%26CK-orange">
</p>

<p align="center">
<img src="https://img.shields.io/badge/Windows%2010-0078D6?logo=windows&logoColor=white">
<img src="https://img.shields.io/badge/Kali%20Linux-557C94?logo=kalilinux&logoColor=white">
<img src="https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu&logoColor=white">
<img src="https://img.shields.io/badge/pfSense-212121?logo=pfsense&logoColor=white">
<img src="https://img.shields.io/badge/Suricata-D3212D">
</p>

---

## 📖 Project Summary

The **SOC Blue Team Lab** simulates a real-world enterprise security operations
environment — built during a 90-day Blue Team SOC Analyst internship at Cyberster.
It integrates SIEM deployment, network segmentation, intrusion detection, threat
intelligence, incident response, and digital forensics into one continuous,
hands-on workflow.

Unlike a single-purpose tutorial lab, this project evolved week over week: starting
from basic log analysis, progressing through detection engineering and network
segmentation with pfSense, and culminating in full incident response and DFIR
capstone investigations.

---

## 🎯 Objectives

- ✅ Build a multi-VM SOC lab with segmented, routed network topology
- ✅ Deploy and tune Wazuh SIEM across Windows and Linux endpoints
- ✅ Develop custom detection rules (Wazuh + Suricata)
- ✅ Integrate threat intelligence feeds mapped to MITRE ATT&CK
- ✅ Conduct incident response following NIST SP 800-61
- ✅ Perform digital forensics investigations (Volatility 3, browser/LNK/USB analysis)

---

## 🖥️ Lab Architecture

| Component | Role | IP |
|---|---|---|
| Kali Linux | Attacker / testing | 192.168.56.102 |
| Ubuntu Server (Wazuh Manager) | SIEM, FIM, log analysis | 192.168.56.10 |
| Windows 11 | Monitored endpoint (Wazuh agent) | agent |
| pfSense | Firewall / network segmentation | gateway |

---

## 📂 Repository Structure

| Folder | Contents |
|---|---|
| `01-Lab-Setup` | VM builds, networking, base configuration |
| `02-Wazuh-Deployment` | Agent enrollment, FIM, event log monitoring |
| `03-Detection-Engineering` | Custom Wazuh & Suricata rules |
| `04-Network-Segmentation` | pfSense setup, routing, firewall rules |
| `05-Threat-Intelligence` | VirusTotal, URLhaus, MITRE ATT&CK mapping |
| `06-Incident-Response` | NIST 800-61 IR reports, insider threat simulation |
| `07-Digital-Forensics` | Browser/LNK/USB forensics, Volatility 3 capstone |
| `docs-archive` | Full week-by-week internship reports (raw archive) |

---

## 🔍 Notable Findings

- **VirusTotal API v2 → v3 patch** — Wazuh 4.14.5's bundled `virustotal.py`
  integration script targets the deprecated VirusTotal API v2 endpoint, which
  fails against valid v3 keys. Identified and patched it to use the v3 endpoint
  with correct header-based authentication — documented as a genuine
  visibility-gap finding.
- **Silent detection rule failure** — A custom Wazuh rule failed to fire with no
  error, caused by referencing the wrong FIM event field (`file` instead of the
  actual `syscheck.path`). Found by inspecting the raw alert JSON directly
  rather than trusting a clean service restart log.
- **pfSense boot failure on VirtualBox** — The pfSense VM refused to boot into
  64-bit long mode because Hyper-V held exclusive access to VT-x. Resolved by
  setting VirtualBox's paravirtualization interface to Hyper-V and correcting
  the guest OS type to 64-bit FreeBSD.
---

## 🧠 Skills Demonstrated

`MITRE ATT&CK` `NIST SP 800-61` `Detection Engineering` `Network Segmentation`
`SIEM/IDS Deployment` `Digital Forensics` `Volatility 3` `Threat Intelligence`

### 🧩 Skills Breakdown

- **Network Analysis** — Wireshark (ARP, DNS, HTTP, QUIC, ICMPv6), Suricata IDS
  in af-packet mode with custom rules, validated via offline PCAP replay
- **SIEM / Detection Engineering** — Wazuh agent deployment, File Integrity
  Monitoring, custom MITRE-mapped detection rules, Active Response automation
  (firewall-drop on SSH brute-force)
- **Threat Intelligence** — VirusTotal API v3 enrichment, Abuse.ch/URLhaus feeds
- **Malware Analysis** — Dynamic analysis via ANY.RUN sandbox, IOC/IOA extraction
- **Incident Response** — NIST SP 800-61 IR plans, insider threat simulation
  (staging, encoding, exfiltration, deletion), CyberChef decoding
- **Digital Forensics** — Browser forensics (Chrome/Edge/Firefox/Brave), LNK
  file analysis (LECmd), USB device forensics, memory forensics (Volatility 3),
  disk image analysis (Autopsy, .E01)
- **Network Security** — pfSense firewall configuration, GeoIP blocking
  (ipset/iptables), network segmentation

## 🔗 Background

Completed as part of the **Cyberster SOC Analyst Internship (Batch 2)**. Weekly
progress also documented on [LinkedIn](https://www.linkedin.com/in/hanzallah-io/).