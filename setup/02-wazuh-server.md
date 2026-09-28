# 03 - Wazuh server

**Purpose:** Set the SIEM (Manager + Indexer + Dashboard, all in one)

**Prerequisites:** 00, 01

## Steps
1. Install the Ubuntu 24.04 Server, give it 4gb of memory, 2 vCPUs, 60GB dynamic disk, have both adapter 1 and 2 enabled for install, whether or not to install OpenSSH is up to you.
2. Set the static IP to 192.168.56.5, you can do this during installation in the network config, or use netplan once the OS has booted.
3. Install Wazuh 4.14 all-in-one:

   curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a

   
   It should install smoothly as the VM has 4gb of memory but if it refuses to install add the -i flag: 

   sudo bash ./wazuh-install.sh -a -i.


   The -i flag forces the installer to ignore the minimum system requirements, an alternative is too bump up the memory temporarily during installation.
5. Once Wazuh installs it will output a dashboard password, save the password in a password manager.
6. Verify that everything works by opening the dashboard on your host, https://192.168.56.5, log in as admin using the dashboard password.
