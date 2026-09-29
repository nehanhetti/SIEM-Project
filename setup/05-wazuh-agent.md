# 05 - Wazuh agent on WS01

**Purpose:** Setup the agent on WS01 so we can get the client's telemtry, including Sysmon events.

**Prerequisites:** 02, 03, 04

## Steps
1. From the Wazuh dashboard, go to the agents screen and select deploy new agent
2. Set the OS to Windows, Server IP to 192.168.56.5, give it the agent name WS01, and copy the generated command.
3. Run the command in an elevated powershell on WS01, WS01 should show active on the dashboard within a few minutes.
4. Connect sysmon to the agent, go to C:\Program Files (x86)\ossec-agent\ossec.conf on WS01, and add:
   ```xml
   <localfile>
     <location>Microsoft-Windows-Sysmon/Operational</location>
     <log_format>eventchannel</log_format>
   </localfile>
   ```
5. Then Restart-Service WazuhSvc
6. You can test its working by opening an application on WS01 and looking for a process creation event in the dashboard. 
