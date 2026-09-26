\# 03 — Detection Engineering



Custom detection rules built and tuned across the SIEM/IDS stack.



\- `wazuh/local\_rules.xml` — custom Wazuh detections, including a persistence

&#x20; and process/command-line ruleset debugged for rule ID collisions and

&#x20; validated with `wazuh-logtest`

\- `suricata/local.rules` — custom Suricata IDS rules for af-packet mode,

&#x20; tested via offline PCAP replay

\- Includes a fix for a silent rule failure caused by referencing the wrong

&#x20; FIM field name (`file` vs. the correct `syscheck.path`)

