# Enterprise Network Design (CCNA)

<img width="1366" height="673" alt="Topology" src="https://github.com/user-attachments/assets/5fbf9700-0a6f-442e-9190-9396c71950cf" />


---

## Overview

This project demonstrates the design and implementation of a redundant Enterprise Network based on Cisco's Three-Tier Architecture using Cisco Packet Tracer.

The network was built to simulate a real enterprise environment by combining Layer 2 and Layer 3 technologies while providing scalability, redundancy, and high availability.

The implementation includes VLAN segmentation, dynamic routing, gateway redundancy, EtherChannel, NAT, DHCP, and multiple routing techniques.

---

## Network Architecture

The network follows Cisco's Three-Tier Architecture.

- **Core Layer**
  - 2 Cisco 1841 Routers
  - Provides connectivity between distribution switches and external networks.

- **Distribution Layer**
  - 2 Cisco 3560 Multilayer Switches
  - Performs Layer 3 routing.
  - Hosts the SVIs.
  - Provides HSRP gateway redundancy.
  - Runs OSPF.

- **Access Layer**
  - 3 Cisco 2960 Switches
  - Connects end devices.
  - Provides VLAN access.
  - Uses trunk links toward the distribution layer.

---

## Devices

| Device | Quantity |
|----------|---------:|
| Cisco 1841 Router | 2 |
| Cisco 3560 Multilayer Switch | 2 |
| Cisco 2960 Switch | 3 |
| PCs | 4 |
| Servers | 2 |
|outside ser|1 |
---

## Technologies Used

| Technology | Status |
|------------|:------:|
| VLAN Segmentation | ✅ |
| Trunk Links | ✅ |
| EtherChannel (LACP) | ✅ |
| Switch Virtual Interfaces (SVI) | ✅ |
| Rapid PVST | ✅ |
| HSRP | ✅ |
| Loopback Interfaces | ✅ |
| OSPF | ✅ |
| Static Routing | ✅ |
| Floating Static Route | ✅ |
| NAT Overload (PAT) | ✅ |
| DHCP | ✅ |

---

# VLAN Configuration

## VLAN Design

| VLAN | Name | Subnet | Virtual Gateway |
|------|------|----------------|----------------|
| 10 | Sales | 192.168.10.0/25 | 192.168.10.124 |
| 20 | IT | 192.168.20.0/26 | 192.168.20.60 |
| 30 | Development | 192.168.30.0/27 | 192.168.30.28 |

---

## Purpose

The network was segmented into three VLANs to isolate departments, reduce broadcast traffic, improve security, and simplify network management.

---

## Configuration

```
vl 10 
name seals
ex
vl 20 
name Dev
ex
vl 30
name IT
ex

```

---

## Verification

ضع صور:

- show vlan brief (SW1)
  <img width="574" height="59" alt="SW1 V" src="https://github.com/user-attachments/assets/f853bc7b-5fa0-4b6b-a729-f59510849871" />
- show vlan brief (SW2)
<img width="595" height="69" alt="SW2 Vl" src="https://github.com/user-attachments/assets/76242da1-79c9-4c21-9280-1c8df5646cb8" />
- show vlan brief (SW3)
<img width="556" height="71" alt="SW3 V" src="https://github.com/user-attachments/assets/b5936596-ed29-4717-87fc-301d0bac8d18" />
- show vlan brief (MLS4)
 <img width="688" height="102" alt="SVI MLS4" src="https://github.com/user-attachments/assets/60e8c7df-0527-4d9d-b157-0ecb5c6a4e41" />
- show vlan brief (MLS5)
<img width="693" height="111" alt="SVI MLS5" src="https://github.com/user-attachments/assets/83c82224-473f-4be4-b44f-38b47b819eab" />

---
# Trunk Configuration

## Purpose

Trunk links were configured to allow multiple VLANs to traverse between the Access Layer and the Distribution Layer.

Each trunk carries only the required VLANs in order to reduce unnecessary broadcast traffic.

---

## Access Switches

### SW1

```bash
interface range FastEthernet0/3 , FastEthernet0/8
 switchport mode trunk
 switchport trunk allowed vlan 10,20
```

---

### SW2

```bash
interface range FastEthernet0/4 - 5
 switchport mode trunk
 switchport trunk allowed vlan 10,30
```

---

### SW3

```bash
interface range FastEthernet0/6 - 7
 switchport mode trunk
 switchport trunk allowed vlan 20,30
```

---

## Verification

```bash
show interfaces trunk
```


- SW1
 <img width="574" height="59" alt="SW1 V" src="https://github.com/user-attachments/assets/2c56e2c7-48e0-4763-bd5f-a4d27843b8b4" />
- SW2
<img width="595" height="69" alt="SW2 Vl" src="https://github.com/user-attachments/assets/6ff1bcdb-be42-4e53-b89a-b11435255f6e" />
- SW3
<img width="556" height="71" alt="SW3 V" src="https://github.com/user-attachments/assets/a9dd79c1-c926-4d5a-88fa-a8b683819963" />

---

# EtherChannel (LACP)

## Purpose

EtherChannel was implemented between the two Distribution Switches to increase bandwidth and provide link redundancy.

LACP (IEEE 802.3ad) was used to dynamically negotiate the EtherChannel.

---

## Configuration

### MLS4

```bash
interface range FastEthernet0/10 - 11
 channel-group 1 mode active

interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
```

---

### MLS5

```bash
interface range FastEthernet0/10 - 11
 channel-group 1 mode passive

interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
```

---

## Verification

```bash
show etherchannel summary
```

```bash
show interfaces trunk
```

Place screenshots for:

- MLS4 EtherChannel Summary
 <img width="1366" height="768" alt="show ethernet summary(MLS4)" src="https://github.com/user-attachments/assets/f1d0a27d-6eec-4d08-8237-6e5cbc32e0bc" />
- MLS5 EtherChannel Summary
<img width="1366" height="768" alt="show ethernet summary(MLS5)" src="https://github.com/user-attachments/assets/92b4dc08-db28-458e-88eb-4f2f57f87934" />
- MLS4 Trunk
<img width="1366" height="768" alt="show interfaces trunk(MLS4)" src="https://github.com/user-attachments/assets/5ff23c38-687c-4d13-b885-180260b92e61" />
- MLS5 Trunk
<img width="1366" height="768" alt="show interfaces trunk(MLS5)" src="https://github.com/user-attachments/assets/a5ef14a5-173f-4f1a-99b2-95df2ee04327" />

---

## Design Notes

- LACP was selected instead of the "on" mode to provide dynamic negotiation.
- EtherChannel increases available bandwidth between the Distribution Switches.
- If one physical link fails, traffic continues through the remaining active links.
- Port-channel1 operates as a single logical trunk link carrying all required VLANs.
# Switch Virtual Interfaces (SVI)

## Purpose

Switch Virtual Interfaces (SVIs) were configured on both Distribution Multilayer Switches to provide Layer 3 routing between VLANs.

IP routing was enabled to allow Inter-VLAN communication directly on the multilayer switches.

---

## MLS4 Configuration

```bash
ip routing

interface Vlan10
 ip address 192.168.10.126 255.255.255.128
 no shutdown

interface Vlan20
 ip address 192.168.20.62 255.255.255.192
 no shutdown

interface Vlan30
 ip address 192.168.30.30 255.255.255.224
 no shutdown
```

---

## MLS5 Configuration

```bash
ip routing

interface Vlan10
 ip address 192.168.10.125 255.255.255.128
 no shutdown

interface Vlan20
 ip address 192.168.20.61 255.255.255.192
 no shutdown

interface Vlan30
 ip address 192.168.30.29 255.255.255.224
 no shutdown
```

---

## Verification

```bash
show ip interface brief
```

Place screenshots for:

- MLS4 SVI Interfaces

- MLS5 SVI Interfaces

---

# Rapid PVST

## Purpose

Rapid Per-VLAN Spanning Tree (Rapid PVST) was implemented to eliminate Layer 2 loops while providing fast convergence after topology changes.

Root Bridge priorities were manually configured to achieve load balancing between the Distribution Switches.

---

## Root Bridge Design

| VLAN | Root Bridge | Secondary Root |
|-------|-------------|----------------|
| VLAN 10 | MLS4 | MLS5 |
| VLAN 20 | MLS5 | MLS4 |
| VLAN 30 | MLS5 | MLS4 |

---

## Configuration

### MLS4

```bash
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary
spanning-tree vlan 30 root secondary
```

---

### MLS5

```bash
spanning-tree vlan 10 root secondary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
```

---

## Verification

```bash
show spanning-tree
```

Place screenshots for:

- MLS4 STP
```
MLS4#show spanning-tree
VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    4097
             Address     0002.16C6.2EC2
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4097  (priority 4096 sys-id-ext 1)
             Address     0002.16C6.2EC2
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/3            Desg FWD 19        128.3    P2p
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/7            Desg FWD 19        128.7    P2p
Po1              Desg FWD 12        128.27   P2p

VLAN0010
  Spanning tree enabled protocol rstp
  Root ID    Priority    4106
             Address     0002.16C6.2EC2
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4106  (priority 4096 sys-id-ext 10)
             Address     0002.16C6.2EC2
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/3            Desg FWD 19        128.3    P2p
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/7            Desg FWD 19        128.7    P2p
Po1              Desg FWD 12        128.27   P2p

VLAN0020
  Spanning tree enabled protocol rstp
  Root ID    Priority    4116
             Address     00E0.F966.9432
             Cost        12
             Port        27(Port-channel1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    8212  (priority 8192 sys-id-ext 20)
             Address     0002.16C6.2EC2
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/3            Desg FWD 19        128.3    P2p
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/7            Desg FWD 19        128.7    P2p
Po1              Root FWD 12        128.27   P2p

VLAN0030
  Spanning tree enabled protocol rstp
  Root ID    Priority    4126
             Address     00E0.F966.9432
             Cost        12
             Port        27(Port-channel1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    8222  (priority 8192 sys-id-ext 30)
             Address     0002.16C6.2EC2
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/3            Desg FWD 19        128.3    P2p
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/7            Desg FWD 19        128.7    P2p
Po1              Root FWD 12        128.27   P2p
```
- MLS5 STP
```
MLS5#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    4097
             Address     0002.16C6.2EC2
             Cost        12
             Port        27(Port-channel5)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    8193  (priority 8192 sys-id-ext 1)
             Address     00E0.F966.9432
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/6            Desg FWD 19        128.6    P2p
Fa0/5            Desg FWD 19        128.5    P2p
Fa0/8            Desg FWD 19        128.8    P2p
Po5              Root FWD 12        128.27   P2p

VLAN0010
  Spanning tree enabled protocol rstp
  Root ID    Priority    4106
             Address     0002.16C6.2EC2
             Cost        12
             Port        27(Port-channel5)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    8202  (priority 8192 sys-id-ext 10)
             Address     00E0.F966.9432
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/6            Desg FWD 19        128.6    P2p
Fa0/5            Desg FWD 19        128.5    P2p
Fa0/8            Desg FWD 19        128.8    P2p
Po5              Root FWD 12        128.27   P2p

VLAN0020
  Spanning tree enabled protocol rstp
  Root ID    Priority    4116
             Address     00E0.F966.9432
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4116  (priority 4096 sys-id-ext 20)
             Address     00E0.F966.9432
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/6            Desg FWD 19        128.6    P2p
Fa0/5            Desg FWD 19        128.5    P2p
Fa0/8            Desg FWD 19        128.8    P2p
Po5              Desg FWD 12        128.27   P2p

VLAN0030
  Spanning tree enabled protocol rstp
  Root ID    Priority    4126
             Address     00E0.F966.9432
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4126  (priority 4096 sys-id-ext 30)
             Address     00E0.F966.9432
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/6            Desg FWD 19        128.6    P2p
Fa0/5            Desg FWD 19        128.5    P2p
Fa0/8            Desg FWD 19        128.8    P2p
Po5              Desg FWD 12        128.27   P2p
```
---

# HSRP (Gateway Redundancy)

## Purpose

Hot Standby Router Protocol (HSRP) was configured to provide default gateway redundancy for all VLANs.

Load balancing was achieved by distributing the Active Gateway role across the two Distribution Switches.

---

## Virtual Gateway Addresses

| VLAN | Virtual Gateway |
|-------|-----------------|
| VLAN 10 | 192.168.10.124 |
| VLAN 20 | 192.168.20.60 |
| VLAN 30 | 192.168.30.28 |

---

## Active / Standby Design

| VLAN | Active | Standby |
|-------|---------|----------|
| VLAN 10 | MLS4 | MLS5 |
| VLAN 20 | MLS5 | MLS4 |
| VLAN 30 | MLS5 | MLS4 |

---

## MLS4 Configuration

```bash
interface Vlan10
 standby 10 ip 192.168.10.124
 standby 10 priority 120
 standby 10 preempt

interface Vlan20
 standby 20 ip 192.168.20.60
 standby 20 priority 80

interface Vlan30
 standby 30 ip 192.168.30.28
 standby 30 priority 80
```

---

## MLS5 Configuration

```bash
interface Vlan10
 standby 10 ip 192.168.10.124
 standby 10 priority 100

interface Vlan20
 standby 20 ip 192.168.20.60
 standby 20 priority 120
 standby 20 preempt

interface Vlan30
 standby 30 ip 192.168.30.28
 standby 30 priority 120
 standby 30 preempt
```

---

## Verification

```bash
show standby brief
```

Place screenshots for:

- MLS4 HSRP Status

- MLS5 HSRP Status

---

## Design Notes

- HSRP provides gateway redundancy.
- A virtual IP address is used as the default gateway for all end devices.
- Load balancing is achieved by distributing the Active HSRP role across VLANs.
- If one Distribution Switch fails, the standby device automatically becomes Active with minimal interruption.
# Loopback Interfaces

## Purpose

Loopback interfaces were configured on all Layer 3 devices to provide stable Router IDs for OSPF.

Unlike physical interfaces, Loopback interfaces remain operational unless they are manually shut down, making them ideal for Router IDs.

---

## Configuration

### R1

```bash
interface Loopback0
 ip address 10.255.0.1 255.255.255.255
```

---

### R2

```bash
interface Loopback0
 ip address 10.255.0.2 255.255.255.255
```

---

### MLS4

```bash
interface Loopback0
 ip address 10.255.0.4 255.255.255.255
```

---

### MLS5

```bash
interface Loopback0
 ip address 10.255.0.5 255.255.255.255
```

---

## Verification

```bash
show ip interface brief
```

Place screenshots for:

- R1
- R2
- MLS4
- MLS5

---

# OSPF (Dynamic Routing)

## Purpose

Open Shortest Path First (OSPF) was configured to dynamically exchange routing information between the Core and Distribution Layers.

All Layer 3 devices participate in OSPF Area 0.

Loopback interfaces were used as Router IDs to ensure consistent OSPF identification.

---

## OSPF Topology

| Device | Router ID |
|----------|-----------|
| R1 | 10.255.0.1 |
| R2 | 10.255.0.2 |
| MLS4 | 10.255.0.4 |
| MLS5 | 10.255.0.5 |

---

## Configuration

### MLS4

```bash
router ospf 1
 router-id 10.255.0.4

 network 172.16.0.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.127 area 0
 network 192.168.20.0 0.0.0.63 area 0
 network 192.168.30.0 0.0.0.31 area 0
```

---

### MLS5

```bash
router ospf 1
 router-id 10.255.0.5

 network 172.16.0.8 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.127 area 0
 network 192.168.20.0 0.0.0.63 area 0
 network 192.168.30.0 0.0.0.31 area 0
```

---

### R1

```bash
router ospf 1
 router-id 10.255.0.1

 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.4 0.0.0.3 area 0
 network 8.8.8.8 0.0.0.0 area 0
```

---

### R2

```bash
router ospf 1
 router-id 10.255.0.2

 network 172.16.0.4 0.0.0.3 area 0
 network 172.16.0.8 0.0.0.3 area 0
```

---

## Verification

```bash
show ip ospf neighbor
```

```bash
show ip route ospf
```

Place screenshots for:

- MLS4
- MLS5
- R1
- R2

---

# Static Routing

## Purpose

Static routes were configured for specific remote networks to provide deterministic routing paths where dynamic routing was not required.

---

## Configuration

### MLS4

```bash
ip route 1.1.1.0 255.255.255.252 172.16.0.1
ip route 172.16.0.4 255.255.255.252 172.16.0.1
```

---

### MLS5

```bash
ip route 1.1.1.0 255.255.255.252 172.16.0.9
```

---

### R2

```bash
ip route 1.1.1.0 255.255.255.252 172.16.0.5
ip route 192.168.10.0 255.255.255.128 172.16.0.5
```

---

## Verification

```bash
show ip route static
```

Place screenshots for:

- MLS4
- MLS5
- R2

---


# NAT Overload (PAT)

## Purpose

Network Address Translation (PAT) was configured on R1 to allow multiple private networks to access external networks using a single public IP address.

---

## Configuration

```bash
access-list 1 permit 192.168.10.0 0.0.0.127
access-list 1 permit 192.168.20.0 0.0.0.63
access-list 1 permit 192.168.30.0 0.0.0.31

interface FastEthernet0/0
 ip nat inside

interface FastEthernet0/1
 ip nat inside

interface Ethernet0/1/0
 ip nat outside

ip nat inside source list 1 interface Ethernet0/1/0 overload
```

---

## Verification

```bash
show access-lists
```

```bash
show ip nat statistics
```

```bash
show ip nat translations
```

Place screenshots for each command.

---

# DHCP

## Purpose

DHCP was configured to automatically assign IP addresses, subnet masks, default gateways, and DNS information to end devices.

---

## Configuration

Place your DHCP pools here.

---

## Verification

```bash
show ip dhcp binding
```

```bash
show ip dhcp pool
```
# Testing & Verification

The entire network was tested after completing the configuration to verify connectivity, routing, redundancy, and network services.

---

## Layer 2 Verification

The following tests were performed successfully:

- ✅ VLAN communication
- ✅ Trunk links
- ✅ EtherChannel operation
- ✅ Rapid PVST Root Bridge election

### Verification Commands

```bash
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree
```

Place screenshots for each command.

---

## Layer 3 Verification

The following Layer 3 services were verified successfully:

- ✅ Inter-VLAN Routing
- ✅ OSPF Neighbor Adjacency
- ✅ OSPF Routing Table
- ✅ Static Routes
- ✅ Floating Static Route

### Verification Commands

```bash
show ip ospf neighbor
show ip route
show ip route ospf
show ip route static
```

Place screenshots for each command.

---

## Gateway Redundancy Verification

HSRP was tested by verifying Active and Standby roles across all VLANs.

### Verification Command

```bash
show standby brief
```

Place screenshots showing:

- VLAN 10 Active on MLS4
- VLAN 20 Active on MLS5
- VLAN 30 Active on MLS5

---

## Network Services Verification

The following services were successfully verified:

### DHCP

```bash
show ip dhcp binding
show ip dhcp pool
```

---

### NAT

```bash
show ip nat statistics
show ip nat translations
```

---

## End-to-End Connectivity

Connectivity tests were performed between different VLANs to verify complete network functionality.

Example tests:

- PC1 → PC2
- PC1 → Server
- PC2 → PC6
- VLAN 10 → VLAN 20
- VLAN 10 → VLAN 30
- VLAN 20 → VLAN 30

Place screenshots of successful ping tests.

---

# Troubleshooting

During the implementation, several issues were identified and resolved.

| Issue | Cause | Solution |
|--------|-------|----------|
| VLAN communication failed | Incorrect trunk configuration | Corrected allowed VLANs |
| HSRP Active/Standby roles were incorrect | HSRP priority values | Adjusted priorities and enabled preemption |
| OSPF neighbors were not forming | Incorrect network statements | Updated OSPF configuration |
| NAT translations were not created | No outbound traffic generated | Generated traffic and verified translations |
| DHCP addresses were not assigned | VLAN configuration issue | Corrected VLAN assignment and DHCP configuration |

---

# Skills Demonstrated

This project demonstrates practical experience with:

- Enterprise Network Design
- Cisco Three-Tier Architecture
- VLAN Segmentation
- Layer 2 Switching
- Layer 3 Switching
- Inter-VLAN Routing
- EtherChannel (LACP)
- Rapid PVST
- HSRP
- OSPF
- Static Routing
- Floating Static Routing
- Loopback Interfaces
- NAT Overload (PAT)
- DHCP Configuration
- Network Verification
- Network Troubleshooting

---

# Project Summary

This project demonstrates the implementation of a redundant Enterprise Network using Cisco Packet Tracer and Cisco IOS technologies.

The network was designed following the Cisco Three-Tier Architecture to provide scalability, redundancy, and high availability.

Implemented technologies include:

- VLAN Segmentation
- Trunk Links
- EtherChannel (LACP)
- Switch Virtual Interfaces (SVIs)
- Rapid PVST
- HSRP Gateway Redundancy
- Loopback Interfaces
- OSPF Dynamic Routing
- Static Routing
- Floating Static Routes
- NAT Overload (PAT)
- DHCP Services

The project was fully validated using Cisco IOS verification commands and end-to-end connectivity testing.

---

## Repository Contents

```
Enterprise-Network-Design/
│
├── README.md
├── Enterprise-Network.pkt
└── Images/
    ├── Topology.png
    ├── VLAN/
    ├── Trunk/
    ├── EtherChannel/
    ├── SVI/
    ├── STP/
    ├── HSRP/
    ├── Loopback/
    ├── OSPF/
    ├── Static-Routing/
    ├── NAT/
    ├── DHCP/
    └── Testing/
```

---

## Author

**Amr Alferaidi**

