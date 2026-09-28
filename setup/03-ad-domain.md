# 03 - AD Domain (DC & Client)

**Purpose:** Setup a domain controller with one domain joined client

**Prerequisites:** 00, 01

## DC01

1. Setup the Windows Server 2022 VM, 2gb memory, 2 vCPU, 40gb dynamic disk. Because of my resources I decided to forgo the desktop experience (CLI only) so the server is lighter to run.
2. Set the static IP 192.168.56.10, DNS pointing at itself.
3. Promote the server to domain controller
4. Create accounts (admin and user), add the admin user to the Domain Admins group.

## WS01

1. VM: 4gb memory, 2 vCPU, 60gb dynamic disk, I also installed VBox guest additions for a variety of QOL features.
2. Set the IP to 192.168.56.20, DNS = 192.168.56.10 (DC). Confirm nslookup lab.local returns the DC.
  
   I ran into some problems here, see **Problems and fixes** at the bottom of this file for more details.
5. Once the client can reach the DC, join the domain and reboot.
6. Log in as your user, you can also install RSAT here if you want to remotely admin the DC.

At this point we have our main stack running, all thats left is to install sysmon, the wazuh agents, and atomic red team.

## Problems and fixes

During step 2 of WS01 after setting the client's IP and pointing it at the DC, nslookup would timeout and not return the DC. The first thing to check was whether both machines were connected to host-only network. After confirming both machines had adapter 1, I did a Test-NetConnection 192.168.56.10, which returned DestinationHostUnreachable. After some more diagnosing of the client machine (making sure the client's IP and DNS was properly set) I moved to the DC to check if the IP had been properly set. Running a Get-NetAdapter showed one interface "Ethernet 2." Running a Get-NetIpAddress showed that the IP for the interface was set to '169.254.17.30', during the setup of DC01 I had set the IP of the NAT adapter (Ethernet) and the left the host-only adapter (Ethernet 2) as default. As such, after disabling the NAT adapter the DC didn't have the IP 192.168.56.10.

The fix was to set the IPv4 of the interface alias "Ethernet 2" to 192.168.56.10
