# Suricata IDS Lab – EVE-NG Environment

## Objective

Deploy and configure Suricata as a Network Intrusion Detection System (NIDS) in a simulated enterprise environment using EVE-NG. The lab aims to detect common attacks such as brute-force SSH, ICMP floods, and port scanning by leveraging custom rules and default signatures.

## Lab Environment

- **Platform:** EVE-NG Community Edition
- **Nodes:**
  - GW1: Cisco Router
  - SW1: vIOS Switch (configured with SPAN)
  - Kali Linux: Attacker node
  - Ubuntu Server: Suricata monitoring node
  - VPC: Target node (DHCP enabled)
- **Network:** 192.168.6.0/24
- **Internet access:** Provided through GW1 with NAT and DHCP

## Key Configurations

- **GW1 Router**
  - NAT overload via access-list
  - DHCP pool for 192.168.6.0/24 with exclusions
  - Default route: `0.0.0.0/0 -> 192.168.36.2`

- **SPAN Port on Switch**
  - Mirrored Gi0/2 traffic to interface Gi1/0 (connected to Suricata node)

## Suricata Setup

1. **Interface Configuration**
   - Interface `ens4` (e1) assigned via DHCP: 192.168.6.13
   - Promiscuous mode activated via systemd service

2. **Suricata Installation**
   - Version 6.0.4 installed via PPA
   - `suricata.yaml` updated:
     - `HOME_NET: [192.168.6.0/24]`
     - `af-packet` set to monitor `ens4`

3. **Traffic Rules**
   - Custom rule added for HTTP traffic: `sid:1000002`
   - Detection for SYN-flood attacks simulated via `hping3`
   - Kali Linux detection rule disabled by `sid:2022973` using `disable.conf`

4. **Traffic Generation**
   - ICMP requests using `ping` from VPC
   - HTTP access via browser/curl to Apache2
   - Nmap scanning and SYN-flood attack from Kali

5. **Logging and Analysis**
   - Suricata logs reviewed in `/var/log/suricata/fast.log`
   - Packet capture verified with `tcpdump`
   - Alerts correlated with Wireshark output

## Results

- Successful detection of ICMP, SYN-flood, and HTTP patterns
- Nmap passive scans not detected by default rules
- Disabled rule suppressed noisy detection of Kali Linux

## Conclusions

Suricata, when combined with EVE-NG and proper mirroring (SPAN), provides an ideal testbed for developing and tuning detection rules. Network behavior and IDS effectiveness can be assessed without impacting production environments.
