# 04 - Installing Sysmon on WS01

**Purpose:** Installing sysmon will give us useful endpoint telemtry, we will repeat this process on DC01 later on. 

**Prerequisites:** 03

## Steps

1. Get Sysmon from sysinternals and a sysmon config of your choice (I chose Olaf Hartong's sysmonmodular, see **justification** at the bottom of this page for more info)
2. Install:
   ```powershell
   .\Sysmon64.exe -accepteula -i sysmonconfig.xml
   ```
3. You can confirm Sysmon installed correctly by checking Event Viewer. (Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational)

## Justification
Olaf's sysmonmodular maps every config rule to MITRE ATT&CK technique ids. As we will be using Atomic Red Team, the attack technique id will be directly in the SIEM logs. Making it very easy to build dashboards, alerts and verification rules.
