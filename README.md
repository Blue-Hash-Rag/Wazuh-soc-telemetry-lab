# Wazuh-soc-telemetry-lab
A resource-optimized SOC and SIEM home lab simulating cyber attacks and analyzing telemetry logs within a virtualized sandbox.
# Resource-Optimized SOC & SIEM Telemetry Lab

## Overview
An enterprise-grade Security Operations Center (SOC) home lab designed, deployed, and tested within a constrained hardware environment (8 GB Host RAM). This project demonstrates the deployment of a centralized Wazuh SIEM manager, an active Windows telemetry endpoint, and simulated adversarial attack vectors using Kali Linux.

## Key Technical Achievements
* **Advanced Resource Optimization:** Successfully reconfigured the Java Virtual Machine (JVM) heap limits (`-Xms512m / -Xmx512m`) and engineered a 2GB Linux swap space to stabilize the Wazuh Indexer on low-compute infrastructure, preventing kernel Out-Of-Memory (OOM) failures.
* **Telemetry Pipeline Configuration:** Installed and securely registered the lightweight Wazuh MSI agent on a Windows endpoint via PowerShell, facilitating continuous security event forwarding.
* **Network & Security Tuning:** Configured custom VMware guest isolation profiles, host-only/NAT adapters, and adjusted local Windows Defender firewall profiles to allow secure multi-node communication.

## Network Architecture
* **SIEM Manager:** Wazuh Server 4.x (Linux) | 2.5 GB RAM Baseline (IP: 192.168.x.x)
* **Endpoint (Victim):** Windows 10/11 | 1.5 GB RAM Baseline (Performance Optimized)
* **Attacker Profile:** Kali Linux | 1.5 GB RAM Baseline (Staggered Execution Mode)

## Threat Simulation & Incident Handling Case Study

### Incident: Credential Brute-Force Targeting
During an adversarial simulation exercise, the infrastructure successfully intercepted and parsed a rapid-fire authentication attack originating from the Kali Linux node targeting the Windows endpoint.

#### 1. SIEM Detection Metrics
* **Triggered Alert:** Wazuh Rule ID 60122 (Multiple failed login attempts)
* **Severity Level:** High (Level 10+)
* **Telemetry Source:** Windows Event Channel (Security)

#### 2. Raw Log Evidence & Analysis
The Wazuh decoder parsed the following telemetry fields from the incoming stream, identifying a clear malicious pattern:

```text
[Timestamp]: 2026-05-31T01:45:12.104Z
[Provider]: Microsoft-Windows-Security-Auditing
[EventID]: 4625 
[Description]: An account failed to log on.
[Failure Reason]: Unknown user name or bad password.
[Target Account]: Administrator
[Source Network Address]: 192.168.x.x (Kali Linux Attacker IP)
[Logon Type]: 3 (Network Logon attempt)
