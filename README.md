# network-threat-detection-suricata
Practical lab using Suricata IDS in an EVE-NG virtual lab. Includes detection rule creation, attack simulation with Kali Linux, and alert analysis. SOC and cybersecurity training environments.

# Network Threat Detection with Suricata

This repository presents a complete lab project focused on deploying and configuring the Suricata Intrusion Detection System (IDS) within a virtual environment built on EVE-NG. The goal is to simulate real-world attack traffic (e.g., brute-force, SYN floods, passive scans) and build detection rules that allow Suricata to identify these threats.

## Lab Overview

- Platform: EVE-NG Community Edition
- IDS: Suricata v6.0.4 on Ubuntu Server 22.04
- Attacker: Kali Linux
- Targets: VPC, Apache2 services
- Network span monitoring via vIOS switch (SPAN port)

## Simulated Threats

- SSH brute-force (Hydra)
- HTTP monitoring (custom rules)
- ICMP pings and floods
- SYN flood (hping3)
- Passive port scanning (nmap)

## Repository Structure
├── detection_rules_en.md       # Report in English
├── detection_rules_es.md       # Versión en español
└── screenshots/                # Packet captures and system output

## Highlights

- Detection rules crafted and tested live
- Integration with Wireshark and tcpdump for verification
- Demonstrates Suricata deployment in real-like network topology
- Uses traffic mirroring (SPAN) to monitor multi-host environments

## Author

Bruno Paolo Huamán Vela  (Lima, Peru)
Cybersecurity Student – Ural Federal University (UrFU)  
Focus: Network Security, SOC Monitoring, Threat Detection  
