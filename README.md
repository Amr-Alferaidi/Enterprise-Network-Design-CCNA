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
show vlan brief (SW1)

  <img width="574" height="59" alt="SW1 V" src="https://github.com/user-attachments/assets/f853bc7b-5fa0-4b6b-a729-f59510849871" />

  ---

show vlan brief (SW2)

<img width="595" height="69" alt="SW2 Vl" src="https://github.com/user-attachments/assets/76242da1-79c9-4c21-9280-1c8df5646cb8" />

  ---
show vlan brief (SW3)

<img width="556" height="71" alt="SW3 V" src="https://github.com/user-attachments/assets/b5936596-ed29-4717-87fc-301d0bac8d18" />
  
  ---
show vlan brief (MLS4)

 <img width="688" height="102" alt="SVI MLS4" src="https://github.com/user-attachments/assets/60e8c7df-0527-4d9d-b157-0ecb5c6a4e41" />

  ---
show vlan brief (MLS5)

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


SW1
<img width="501" height="241" alt="SW1Trunk" src="https://github.com/user-attachments/assets/bbafc54d-e8be-48b5-86df-44c9a1c2ec11" />
  
  ---
SW2

<img width="532" height="237" alt="SW2 Trunk" src="https://github.com/user-attachments/assets/63fac8ae-6323-4a4a-bc75-6d1fea3584e8" />
 
  ---
SW3

<img width="526" height="234" alt="SW3 Trunk" src="https://github.com/user-attachments/assets/93428ff6-ea92-405d-90f8-a673def7720f" />

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
<img width="782" height="108" alt="show standby brief(MLS4)" src="https://github.com/user-attachments/assets/0a38428b-10aa-4b0f-a620-43b4180c2271" />

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
- MLS4 HSRP Status
<img width="782" height="108" alt="show standby brief(MLS4)" src="https://github.com/user-attachments/assets/823f5bd8-dac8-41b7-befd-7a936f2e012c" />
- MLS5 HSRP Status
<img width="744" height="106" alt="show standby brief(MLS5)" src="https://github.com/user-attachments/assets/67b11d3a-9432-48b4-92c1-e94453272883" />

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
<img width="1366" height="768" alt="show ip interface brief(R1)" src="https://github.com/user-attachments/assets/a4935ad9-46fa-4454-86c0-a5d057e3bd40" />
- R2
<img width="1366" height="768" alt="show ip interface brief(R2)" src="https://github.com/user-attachments/assets/69f55e92-38be-4370-a5f6-a3e7537fc1c8" />
- MLS4
<img width="1366" height="768" alt="show ip interface brief(MLS4)" src="https://github.com/user-attachments/assets/a1cac00e-3803-4769-bbb7-cdcbfcbb38d9" />
- MLS5
<img width="1366" height="768" alt="show ip interface brief(MLS5)" src="https://github.com/user-attachments/assets/3e693bcd-c489-4a4f-895b-4df06ec7e071" />
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
- MLS4
<img width="1366" height="768" alt="show ip ospf nei(MLS4)" src="https://github.com/user-attachments/assets/0b37159e-022b-40ca-b9c2-acc55626b6a3" />
- MLS5
<img width="1366" height="768" alt="show ip ospf nei(MLS5)" src="https://github.com/user-attachments/assets/eb765e32-824c-4555-bfba-e8fbf15058d5" />
- R1
<img width="1366" height="768" alt="show ip ospf nei(R1)" src="https://github.com/user-attachments/assets/98507815-8707-4d01-b464-6fdc7c180567" />
- R2
<img width="1366" height="768" alt="show ip ospf nei(R2)" src="https://github.com/user-attachments/assets/dedeab21-fbe8-43c8-abcd-ac96e10300fa" />

```bash
show ip route ospf
```
- MLS4
<img width="1366" height="768" alt="show ip ospf(MLS4)" src="https://github.com/user-attachments/assets/94007fa6-c21a-4050-854d-b9548f95367a" />
- MLS5
<img width="1366" height="768" alt="show ip ospf(MLS5)" src="https://github.com/user-attachments/assets/6e0f978c-33c6-40b6-ae00-52e160671788" />
- R1
<img width="1366" height="768" alt="show ip ospf(R1)" src="https://github.com/user-attachments/assets/3c007b35-341b-4d83-9a9c-549771891bf4" />
- R2
<img width="1366" height="768" alt="show ip ospf(R2)" src="https://github.com/user-attachments/assets/ce1d5051-0477-4686-9f06-c792516ea435" />


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

- MLS4
<img width="1366" height="768" alt="show ip route static MLS4" src="https://github.com/user-attachments/assets/987e4b6e-e1ce-41a2-90a0-0cea839883c6" />
- MLS5
<img width="1366" height="768" alt="show ip route static MLS5" src="https://github.com/user-attachments/assets/0eb64904-5948-43d1-aaf7-d14ef0beef05" />
- R2
<img width="1366" height="768" alt="show ip route static R2" src="https://github.com/user-attachments/assets/6146bd67-2826-4fe9-9ee2-9b5b89719ae3" />

---


# NAT Overload (PAT)

## Purpose

NAT Overload (PAT) was configured on R1 to allow devices in VLAN 10, VLAN 20, and VLAN 30 to access the external network using the IP address assigned to the outside interface.

PAT allows multiple inside-local addresses to share a single inside-global address by using unique port numbers.

---

## Configuration

### Inside Networks ACL

```bash
access-list 1 permit 192.168.10.0 0.0.0.127
access-list 1 permit 192.168.20.0 0.0.0.63
access-list 1 permit 192.168.30.0 0.0.0.31
```

### NAT Interfaces

```bash
interface FastEthernet0/0
 ip nat inside

interface FastEthernet0/1
 ip nat inside

interface Ethernet0/1/0
 ip nat outside
```

### PAT Configuration

```bash
ip nat inside source list 1 interface Ethernet0/1/0 overload
```

---

## Verification

```bash
show access-lists 1
show ip nat statistics
show ip nat translations
```
<img width="1366" height="768" alt="show access list 1" src="https://github.com/user-attachments/assets/cd3bbab8-4ab9-42fd-8bc1-164b7d15408e" />

---

<img width="1366" height="768" alt="show ip nat statistics" src="https://github.com/user-attachments/assets/69320987-6510-4d12-bf17-25be4b4633ae" />

---

<img width="1366" height="768" alt="show ip nat translations" src="https://github.com/user-attachments/assets/4a3c97c0-ef2b-4753-b6a5-868eb64a2edb" />


## Testing

From an internal PC:

```text
ping 1.1.1.2
tracert 1.1.1.2
```

After generating traffic, verify the translations again:

```bash
show ip nat translations
show ip nat statistics
```
---

# DHCP

## Purpose

DHCP was configured on **MLS5** to automatically assign IP addresses to devices connected to **VLAN 30**.

Specific IP addresses were excluded from the DHCP pool to reserve them for network infrastructure and the HSRP virtual gateway.

---

## Configuration

```bash
ip dhcp excluded-address 192.168.30.27
ip dhcp excluded-address 192.168.30.28
ip dhcp excluded-address 192.168.30.29
ip dhcp excluded-address 192.168.30.30

ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.224
 default-router 192.168.30.28
```

---

## Reserved Addresses

| IP Address | Purpose |
|------------|---------|
| 192.168.30.27 | Reserved Infrastructure Address |
| 192.168.30.28 | HSRP Virtual Gateway |
| 192.168.30.29 | MLS5 SVI |
| 192.168.30.30 | MLS4 SVI |

---

## Verification

```bash
show running-config 
```
<img width="1366" height="768" alt="show running-config MLS5" src="https://github.com/user-attachments/assets/427c11ee-ce94-42a0-a0a3-062b36270d54" />

```bash
show ip dhcp pool
```
<img width="1366" height="768" alt="show ip dhcp pool MLS5" src="https://github.com/user-attachments/assets/214f55d3-a3b0-4f99-a13f-0408b900dcf8" />

```bash
show ip dhcp binding
```
<img width="1366" height="768" alt="show ip dhcp binding MLS5" src="https://github.com/user-attachments/assets/899a2f1f-c817-47af-86d6-c8c4efb7c6ca" />

---

## Result

Devices connected to VLAN 30 successfully received IP addresses from the configured DHCP pool.

<img width="1366" height="768" alt="DHCP IP Addresing" src="https://github.com/user-attachments/assets/9e21be79-1ca9-4d57-996d-5867a40e5751" />


The reserved infrastructure addresses and the HSRP virtual gateway were excluded from the DHCP scope to prevent IP address conflicts.
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

---

## Gateway Redundancy Verification

HSRP was tested by verifying Active and Standby roles across all VLANs.

### Verification Command

```bash
show standby brief
```

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
- VLAN 10 → VLAN 20
<img width="1366" height="768" alt="Screenshot (75)" src="https://github.com/user-attachments/assets/b8740607-c098-4585-9727-503e34434622" />
- PC1 → PC3
- VLAN 10 → VLAN 30
<img width="1366" height="768" alt="Screenshot (76)" src="https://github.com/user-attachments/assets/07511564-54e7-49ee-9f10-8e5d8b3c7970" />
- PC2 → Ser6
- VLAN 20 → VLAN 30
<img width="1366" height="768" alt="Screenshot (77)" src="https://github.com/user-attachments/assets/9e929737-9511-4639-ba35-0d2a2bf1a0a3" />

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

