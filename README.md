# Home SOC Lab — SIEM Detection Environment
Home lab simulating and detecting attacks using Splunk SIEM

## Overview
A self-built home lab designed to simulate real-world attacks and detect them using a Splunk SIEM. This project was built to gain hands-on experience with the full detection pipeline — from generating attack traffic, to log collection, to writing detection logic — the same core workflow used by SOC analysts.

**Status:** In progress (started Fall 2026)

## Goals
- Simulate common attack techniques in a safe, isolated environment
- Forward logs from a target system into a SIEM
- Write and validate detection rules for each simulated attack
- Document findings the way a SOC analyst would report them

## Architecture

```
[Kali Linux - Attacker]  --->  [Target (Ubuntu) - Victim]  --->  [Splunk Enterprise - SIEM]
      (attacks)                    (generates logs)                (detection & analysis)
```

- **Kali Linux**: attacker machine, used to generate attack traffic (brute force, port scanning, etc.)
- **Target (Ubuntu)**: victim machine, monitored via the Splunk Universal Forwarder
- **Splunk Enterprise**: SIEM used to ingest logs and run detection searches
- All VMs run in VMware Workstation Pro on an isolated internal network, separate from the home network

## Lab Environment

| Machine | Role | OS | IP Address |
|---|---|---|---|
| Kali | Attacker | Kali Linux | 192.168.241.131 |
| Target | Victim | Ubuntu26 Desktop | 192.168.241.130 |
| Host | SIEM (Splunk) | Windows | 192.168.241.1 |

All machines run on an isolated VMware host-only network (VMnet1), separate from the home network.

## Tools Used
- VMware Workstation Pro
- Kali Linux
- Ubuntu Server
- Splunk Enterprise (Free license)
- Splunk Universal Forwarder

## Simulated Attacks & Detections

| # | Attack Simulated | Tool Used | Detection Method | Status |
|---|---|---|---|---|
| 1 | SSH Brute Force | Hydra | Splunk SPL timeline of failed/successful logins by source IP | ✅ Complete |
| 2 | Port Scan | Nmap | Splunk SPL counting distinct destination ports per source IP from UFW block logs | ✅ Complete |

## Detailed Write-ups

### Attack 1: SSH Brute Force
See [`/attacks/1-ssh-bruteforce`](./attacks/1-ssh-bruteforce) for the full command, detection query, and screenshots.

**Summary:** Used Hydra to brute-force SSH on target. Detected the attack in Splunk via a timeline query showing failed logins followed by a successful one from the same source IP.

### Attack 2: Nmap Port Scan
See [`/attacks/2-nmap-portscan`](./attacks/2-nmap-portscan) for the full command, detection query, and screenshots.

**Summary:** Ran an Nmap SYN scan against target. UFW blocked all ports except SSH; detected the scan in Splunk by counting distinct destination ports touched by a single source IP, with the detection threshold calibrated to UFW's actual (rate-limited) logging volume.

## Repository Structure
```
/attacks/1-ssh-bruteforce/  -> Attack command, detection query, notes, screenshots
/attacks/2-nmap-portscan/   -> Nmap command/output, detection query, notes, screenshots
README.md                   -> This file
```


## Author
Henry Craig — [LinkedIn](https://www.linkedin.com/in/henry-craig/)