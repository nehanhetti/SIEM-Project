# 01 - Networking 

**Purpose:** Set up an isolated network before creating any of the VMs

**Prerequisites:** 00

## Model

One host-only network carries all the lab traffic, we will use NAT as a second adapter only when installing/patching on the VMs, the VMs should not have NAT access during attack testing.

## Steps

1. Create a host-only network (In VirtualBox, you can do this in the Host Network Manager)
   - note its subnet (default is 192.168.56.0/24)
   - turn off DHCP, we want to assign static IPs.
2. Each VM gets two adapters:
   - Adapter 1: host-only, always on.
   - Adapter 2: NAT, default to off, only turn on for installation & patching.
3. As we start installing our VMs, we want to assign them their static IPs from the table, and set the DNS to DC01 (192.168.56.10) for every domain member.
