# Networking Homelab

## Overview
This homelab simulates a small enterprise network environment using Cisco devices. The goal is to practice VLANs, DHCP, switch and router configuration, and basic network testing, aligning with entry-level NOC engineer responsibilities.

**Lab Components:**
- Switches: Cisco 2960-X (24-port), 3560G, SME-S2-24
- Router: Cisco 2911
- Firewall: Cisco ASA 5505 (factory reset, pending configuration)
- Host: MacBook Air

---

## Network Topology
- **VLAN 10 – Users / Workstations**
- Router 2911 acts as the **default gateway** and **DHCP server** for VLAN 10
- Switches connect hosts and uplink to the router
- ASA 5505 intended as firewall (currently factory reset)

**Connections:**

| Device      | Interface           | Connects To        | VLAN / Notes                     |
|------------ |------------------ |------------------ |---------------------------------|
| MacBook     | USB-C Ethernet     | 2960-X Gi1/0/1    | VLAN 10, DHCP client             |
| 2960-X      | Gi1/0/25           | Router Gi0/0      | Access/trunk port for lab        |
| 2911 Router | Gi0/0              | 2960-X Gi1/0/25   | IP 10.10.10.1/24, DHCP pool     |
| ASA 5505    | Pending            | 2960-X uplink     | Factory reset, to configure later|

---

## VLAN & Switch Configuration (2960-X)
```text
vlan 10
 name USERS
exit

interface range Gi1/0/1 - 24
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 no shutdown

interface range Gi1/0/25 - 26
 switchport mode trunk
 switchport trunk allowed vlan 10
 no shutdown
