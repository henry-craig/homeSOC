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

## Tools Used
- VMware Workstation Pro
- Kali Linux
- Ubuntu Server
- Splunk Enterprise (Free license)
- Splunk Universal Forwarder

## Simulated Attacks & Detections

| # | Attack Simulated | Tool Used | Detection Method | Status |
|---|---|---|---|---|
