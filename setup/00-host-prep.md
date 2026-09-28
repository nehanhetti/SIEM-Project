# 00 - Host Prep

**Purpose:** Get the laptop ready to run the VMs

## Steps
1. Enable virtualization in UEFI/BIOS
2. Disable memory integriy in Windows Security to fix the Hyper-V conflict. (Alternatively you could use VMWare workstation as your hypervisor which runs on top of Hyper-V)
3. Install Hypervisor (I use VirtualBox but any equivalent will do)
4. Download ISOs:
   - Ubuntu 24.04 LTS Server
   - Windows Server 2022 Eval (180d trial)
   - Windows 11 Enterprice Eval (90d trial)
   - Kali VirtualBox Image
