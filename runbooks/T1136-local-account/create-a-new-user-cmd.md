# T1136.001 Create a new user in a command prompt

## Test description
The atomic will run a script to create a new user in a command prompt. Test can be verified by looking for new user "T1136.001_CMD"

## Event summary table
|Rule ID|Description|Level|Source|Event ID|
|---|---|---|---|---|
|92036|net.exe binary started by cmd shell|3|Sysmon|1|
|92040|net1.exe executed user creation command|12|Sysmon|1|
|60160|Domain Users Group changed|5|Security|4728|
|60109|User account created|8|Security|4720|
|60109|User account enabled|8|Security|4722|
|60110|User account changed|8|Security|4738|
|60170|Users Group changed|5|Security|4732|

## JSON excerpt of level 12 event
```
{
    "agent": {
      "ip": "192.168.56.20",
      "name": "WS01",
      "id": "001"
    },
    "data": {
      "win": {
        "eventdata": {
          "originalFileName": "net1.exe",
          "image": "C:\\\\Windows\\\\System32\\\\net1.exe",
          "parentProcessGuid": "{63cbd3dc-06d3-6acf-9d01-000000001200}",
          "description": "Net Command",
          "processGuid": "{63cbd3dc-06d3-6acf-9e01-000000001200}",
          "processId": "2808",
          "currentDirectory": "C:\\\\Users\\\\nehan.adm\\\\AppData\\\\Local\\\\Temp\\\\",
          "utcTime": "2026-10-14 04:36:35.677",
          "ruleName": "technique_id=T1136.001,technique_name=Local Account",
          "commandLine": "C:\\\\WINDOWS\\\\system32\\\\net1  user /add \\\"T1136.001_CMD\\\" \\\"AtomicLab!2026\\\"",
          "user": "LAB\\\\nehan.adm",
          "parentUser": "LAB\\\\nehan.adm"
        },
        "system": {
          "eventID": "1",
          "channel": "Microsoft-Windows-Sysmon/Operational",
          "computer": "WS01.lab.local",
        }
      }
    },
    "rule": {
      "firedtimes": 1,
      "mail": true,
      "level": 12,
      "description": "net1.exe executed a user creation command",
      "groups": [
        "sysmon",
        "sysmon_eid1_detections",
        "windows"
      ],
      "mitre": {
        "technique": [
          "Local Account"
        ],
        "id": [
          "T1136.001"
        ],
        "tactic": [
          "Persistence"
        ]
      }
    }
```

## Triage
Check if the account name/creation is not expected, "T1136.001_CMD".

Created through net1.exe in TEMP.

LAB/nehan.adm ran the command, possible compromise of nehan.adm credentials.

Account has not logged in yet.

Was not added to a privleged group. (Only in local Users)

Conclusion: Moderate urgency, unusual behaviour.

## Containment
Disable the account immediately. `Disable-LocalUser -Name "AtomicLab!2026"`

Don't delete yet, preserve for forensic review.

## Dealing with admin account
First, check for any active sessions `query session`

If there is evidence of an active session, isolate WS01 from the network. 

Reset nehan.adm password immediately `Set-ADAccountPassword -Identity "nehan.adm" -Reset -NewPassword (ConvertTo-SecureString "<New Secure Password>" -AsPlainText -Force`

Password reset doesn't revoke existing kerberos tickets, force immediate invalidation by disabling and re-enabling the account.

## Test results and telemetry
<img width="1896" height="346" alt="image" src="https://github.com/user-attachments/assets/7c33203b-b074-4249-b62e-adc8caa59d90" />

<sub>**Figure 1: Wazuh events of completed test**</sub>

<img width="953" height="780" alt="image" src="https://github.com/user-attachments/assets/309744a9-bfb2-4c7f-9df9-f3d7c2cb1ec8" />

<sub>**Figure 2: Excerpt of document details for level 12 event**</sub>
