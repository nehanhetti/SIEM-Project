# 06 - Install Sysmon and the Wazuh agent on the domain controller

**Purpose:** Certain attacks will be invisible on the endpoint (WS01) and only show up on the DC. As such, getting telemetry from the DC is especially important when we start using the unmanaged attacker box.

**Prerequisites:** 02, 03 (04, 05 **recommended**)

## Steps

1. The first problem we will run into is that the DC has no internet, and as its a DNS that points at itself, it can't resolve external names, so we can't just run the download commands from the DC.
2. As I already had Sysmon and the config downloaded on WS01, the method I used was to simply copy the files across to the domain controller.
3. Log in on WS01 as an admin, I created a new folder on the DC: \\DC01\C$\Tools,
4. Go through the same process of setting up a wazuh agent, notice that the agent install command can be split up into two parts, downloading the agent, and then installing and starting the agent. Run only the download part onto your client to get wazuh-agent-*.msi.
5. Then, using Copy-Item on Sysmon64.exe, sysmonmodular.xml, and wazuh-agent-*.msi, copy them to the DC. (If you are unable to do this, check that the account you are logged in as is part of the group "Domain Admins" on DC01)
6. Once the files or on your DC, installing everything is the same steps as what we did on WS01.
7. The server core comes with notepad installed, so use that to edit ossec.conf to connect Sysmon to your agent.
8. From there, restart the wazuh service, your second agent should now be active.
