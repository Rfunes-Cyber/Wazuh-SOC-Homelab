# Wazuh SOC Homelab
### Endpoint Monitoring and Detection Investigations
- A hands-on security operations lab demonstrating centralized log collection, Windows endpoint monitoring, and investigation of controlled security events using Wazuh and Microsoft Sysmon.

[View Project Documentation & Reports Here](SOC-Lab-Documentation.pdf)

## Overview
- I built this lab to practice the workflow from infrastructure deployment to alert investigation. Wazuh runs in Docker on an Ubuntu server and monitors two Windows endpoints. A dedicated Windows 11 virtual machine provides an environment for controlled testing.
- The project includes two completed investigations supported by commands, screenshots, and analysis.

## Architecture

| Component | Location | Purpose |
|---|---|---|
| Wazuh Manager | Ubuntu server · Docker | Analyzes events and generates alerts |
| Wazuh Indexer | Ubuntu server · Docker | Indexes and stores security data |
| Wazuh Dashboard | Ubuntu server · Docker | Provides search and investigation tools |
| Wazuh Agent | Main Windows PC | Collects endpoint data |
| Wazuh Agent + Sysmon | Windows 11 VM | Collects Windows logs and process activity |
| Tailscale | Server and administration devices | Enables remote access |

## Technology Stack

- **Infrastructure:** Ubuntu Server, Docker Engine, Docker Compose, Oracle VirtualBox  
- **Monitoring:** Wazuh 4.14.6, Microsoft Sysmon, Windows Event Logs  
- **Administration:** PowerShell, Windows Command Prompt, SSH, Tailscale

   | System | Resources |
   |---|---|
   | Lenovo M710 server | Intel i5 · 16 GB RAM · 256 GB SSD |
   | Windows 11 test VM | 3 vCPUs · 8 GB RAM · 80 GB dynamic disk · NAT |

## Completed Investigations

| Investigation | Activity | Verified Results |
|---|---|---|
| **01 — Authentication Failures** | Repeated incorrect-password attempts against a dedicated local account | Individual failures, correlation of repeated failures, and account-lockout reporting |
| **02 — Account Discovery** | Local account enumeration using `net user` | Sysmon process-creation telemetry and a Wazuh account-discovery alert |

### Investigation 01: Authentication Failures
- Verified Wazuh rules **60122**, **60204**, and **60115** through a controlled local test. Reviewed the correlation rule’s frequency, time window, and source-IP condition to understand why individual failures did not initially produce a repeated-failure alert.
**Outcome:** Windows enforced its account-lockout policy, and Wazuh reported the authentication activity.

### Investigation 02: Account Discovery
- Traced `net user` activity through Sysmon process-creation events and Wazuh rule **92031 — Discovery activity executed**, mapped to **MITRE ATT&CK T1087 — Account Discovery**.
**Outcome:** Matched the endpoint, user, command, parent process, and timestamp to the authorized test.

## Documentation
- The full project documentation includes:
  - Server setup and Wazuh deployment
  - Windows agent enrollment and connectivity checks
  - Sysmon installation and event collection
  - Investigation procedures, findings, and screenshots
  - Troubleshooting notes and test limitations

## Key Takeaways
- Validated the event pipeline from Windows endpoints to the Wazuh dashboard.
- Distinguished individual event detection from correlation of repeated activity.
- Used process and user context to connect alerts to specific actions.
- Recognized that an alert requires investigation before concluding that activity is malicious.
- Documented observed results separately from assumptions and planned work.


## Skills Demonstrated
- Linux and Windows administration
- Containerized SIEM deployment
- Endpoint monitoring and Sysmon integration
- Security event analysis and detection validation
- Alert investigation and evidence collection
- Technical documentation

## Author
- Raymond A. Funes
