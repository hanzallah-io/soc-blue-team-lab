\# SOC Blue Team Lab — Cyberster Internship



A hands-on Blue Team / SOC Analyst lab built during a 90-day internship at Cyberster,

covering SIEM deployment, network segmentation, IDS, detection engineering, and

threat intelligence integration.



\## Architecture



\- \*\*Kali Linux\*\* (192.168.56.102) — attacker/testing machine

\- \*\*Ubuntu Server / Wazuh Manager\*\* (192.168.56.10) — SIEM, log analysis, FIM

\- \*\*Windows 10\*\* — monitored endpoint (Wazuh agent)

\- \*\*pfSense\*\* — firewall/router segmenting all VM traffic (added mid-lab, converting

&#x20; a flat network into a routed, filtered topology)



\## What's in this repo



\- `docs/reports/` — Full week-by-week internship reports (Word docs + PDFs)

\- `detections/wazuh/` — Custom Wazuh detection rules (`local\_rules.xml`)

\- `detections/suricata/` — Custom Suricata IDS rules

\- `scripts/` — Patched `virustotal.py` Wazuh integration script

\- `network/` — pfSense segmentation notes / topology

\- `screenshots/` — Supporting screenshots



\## What was built



\- Wazuh SIEM deployment with agent enrollment, File Integrity Monitoring, and

&#x20; custom detection rules (debugged rule ID collisions, tested via `wazuh-logtest`)

\- Suricata IDS in af-packet mode with custom detection rules, validated via offline

&#x20; PCAP replay

\- pfSense-based network segmentation, converting a flat lab network into a routed,

&#x20; filtered topology

\- Active Response automation (e.g. firewall-drop tied to SSH brute-force detection,

&#x20; MITRE T1110)

\- Threat intelligence integration: VirusTotal FIM enrichment, Abuse.ch/URLhaus feeds,

&#x20; MITRE ATT\&CK mapping

\- Dynamic malware analysis (ANY.RUN sandbox) producing IOCs/IOAs

\- Full incident response documentation following NIST SP 800-61

\- DFIR work: browser forensics, LNK/USB artifact analysis, and a Phase Two capstone

&#x20; digital forensics case (disk image, USB images, RAM dump via Volatility 3)



\## Notable finding



Wazuh 4.14.5's bundled `virustotal.py` integration script targets the deprecated

VirusTotal API v2 endpoint, which fails against valid v3 keys. I identified and

patched the script to use the v3 endpoint (`api/v3/files/{hash}`) with correct

header-based authentication — documented as a genuine visibility-gap finding in the

Days 24–25 report.



\## Skills demonstrated



MITRE ATT\&CK mapping · NIST SP 800-61 incident response · detection engineering ·

network segmentation · SIEM/IDS deployment · digital forensics (Volatility 3, LECmd,

browser artifact analysis)



\## Background



Completed as part of the Cyberster SOC Analyst Internship (Batch 2). Weekly progress

was also documented on \[LinkedIn](https://www.linkedin.com/in/hanzallah-io/).

