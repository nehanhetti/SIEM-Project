# Level 15 92213 PSScriptPolicyTest false positive

## Description

```__PSScriptPolicyTest_<random>.ps1``` is a file created by powershell on launch to check whether AppLocker or WDAC is enforcing a script policy and then deleted. Sysmon flags the file creation and Wazuh rule 92213 flags it because malware commonly stage scripts and exes in Temp. The rule is reasonable but level 15 is Wazuh's highest severity and the alert being triggered on every powershell launch would interfere with the real high severity alerts made by the atomics.

<img width="1897" height="131" alt="Level 15 alerts on the dashboard" src="https://github.com/user-attachments/assets/ae64fa34-74e2-4ed2-bcad-da9cbeed945e"/>

<sub>**Figure 1: Two level 15 alerts from launching PowerShell on the client and domain controller.**</sub>


&nbsp;


<img width="930" height="781" alt="Document details of WS01" src="https://github.com/user-attachments/assets/04bd7e43-fc31-486e-b4ea-4a55f17e824d"/>

<sub>**Figure 2: Document details of the WS01 alert**</sub>

&nbsp;


**WS01 JSON excerpt**
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
          "image": "C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe",
          "processGuid": "{63cbd3dc-255c-6ac2-7202-000000001000}",
          "processId": "7592",
          "utcTime": "2026-10-04 10:07:25.779",
          "targetFilename": "C:\\\\Users\\\\alice.user\\\\AppData\\\\Local\\\\Temp\\\\__PSScriptPolicyTest_oeuam2pu.upg.ps1",
          "ruleName": "technique_id=T1059.001,technique_name=PowerShell",
          "creationUtcTime": "2026-10-04 10:07:25.779",
          "user": "LAB\\\\alice.user"
        },
        "system": {
          "eventID": "11",
          "channel": "Microsoft-Windows-Sysmon/Operational",
        }
      }
    },
    "rule": {
      "firedtimes": 2,
      "id": "92213"
      "mail": true,
      "level": 15,
      "description": "Executable file dropped in folder commonly used by malware",
      "groups": [
        "sysmon",
        "sysmon_eid11_detections",
        "windows"
      ],
      "mitre": {
        "technique": [
          "Ingress Tool Transfer"
        ],
        "id": [
          "T1105"
        ],
        "tactic": [
          "Command and Control"
        ]
      }
    }
```

Interestingly, the technique is flagged as T1105 Ingress Tool Transfer which is technique where an attacker copies external files into a compromised machine, which doesn't quite apply as PowerShell creates the file itself. This is caused by an over generalised rule where any exe or script file dropped inside Temp is flagged as T1105 by Wazuh and sysmon.

## Wazuh Custom Rule:
Added to /var/ossec/etc/rules/local_rules.xml on the Wazuh manager:
``` xml
<group name="sysmon,local,">
  <rule id="100100" level="3">
    <if_sid>92213</if_sid>
    <field name="win.eventdata.image" type="pcre2">(?i)\\\\WindowsPowerShell\\\\v1\.0\\\\powershell\.exe$</field>
    <field name="win.eventdata.targetFilename" type="pcre2">(?i)\\\\AppData\\\\Local\\\\Temp\\\\__PSScriptPolicyTest_[a-z0-9]{8}\.[a-z0-9]{3}\.psm?1$</field>
    <description>Benign: PowerShell Applocker/WDAC policy test</description>
  </rule>
</group>
```

I decided to reduce the level to 3 instead of 0, that way the events stay searchable in the dashboard.

## After Custom Rule:
Verified by starting a new powershell session.
<img width="1906" height="87" alt="Custom rule working" src="https://github.com/user-attachments/assets/5d826b55-19d2-42f2-8426-8c4b11a23dc1"/>
<sub>**Figure 3: New custom rule firing**</sub>

&nbsp;

I also wanted to make sure 92213 still fires. To do this I created a file called drop-test.ps1 in Temp (see below).

<img width="1908" height="92" alt="Verification test to make sure 92213 still fires" src="https://github.com/user-attachments/assets/ae7817be-0406-46b3-9480-0e2c8897dd4a"/>

<sub>**Figure 4: drop test being flagged as 92213**</sub>

## Limitations

- Only covers "powershell.exe"
- Only matches user profile Temp folders.
- Applies to both agents (WS01 and DC01)

