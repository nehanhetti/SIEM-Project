# SIEM-Project
SOC project using Wazuh and Sysmon to detect Active Directory attacks mapped to MITRE ATT&CK.

I built this project to get hands-on practice with SIEM, in particular building and triaging alerts against a domain controller. My main goal is to learn what AD attacks look like in Windows and Sysmon telemetry. I'll be using Atomic Red Team to test and detect MITRE ATT&CK techniques, later on I'll be using an unmanaged attacker box.

## Network Topology:
<img width="773" height="611" alt="Network Topology" src="https://github.com/user-attachments/assets/8f096894-ee50-44e7-9396-b0396ac0ac28" />

## Inventory
### Hosts
| Host | Role | OS / build | vCPU | RAM | Disk | Lab IP | Installed |
|---|---|---|---|---|---|---|---|
| wazuh | SIEM (manager + indexer + dashboard) | Ubuntu 24.04 LTS | 2 | 4 GB | 60 GB | 192.168.56.5 | Wazuh 4.14 all-in-one |
| DC01 | Domain controller, DNS | Windows Server 2022 Core | 2 | 2 GB | 40 GB | 192.168.56.10 | AD DS, DNS, Wazuh Agent, Sysmon |
| WS01 | Monitored endpoint | Windows 11 Enterprise | 2 | 4 GB | 60 GB | 192.168.56.20 | Sysmon, Wazuh agent, Atomic Red Team |
| attacker | Unmanaged attacker | Kali | 1 | 2 GB | 15 GB | 192.168.56.30 | Various tools |

Olaf Hartong's sysmonmodular is my config of choice

### Domain
- Domain: lab.local

#### Seperate privleged and standard accounts: 
- lab\nehan.adm (for domain administration only)
- lab\alice.user (for daily login on WS01)

### Resource budget (16 GB host)
wazuh + DC01 + WS01 ≈ 10 GB of VM RAM,
attacker box will only be powered on when generating attack traffic.

## Detection Coverage (WIP)
| ATT&CK ID | Technique | Detected? | Link |
|---|---|---|---|
| Planned | Planned | Planned | Planned | 

## Key Findings
Planned

## Repo Structure
```
SIEM-Project/
├── README.md
├── setup/          # My build process for setting up the project
├── detections/     # Attack write ups
├── runbooks/       # Alert triaging
```
## Disclaimer
This is an isolated lab, all activity was performed against systems I own.
