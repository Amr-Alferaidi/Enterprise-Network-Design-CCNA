# Enterprise-Network-Design-CCNA

## Devices
2 Core routers,2 dis MLSs ,3 access SWs,6 end Node

## Topology

<img width="1366" height="673" alt="Topology" src="https://github.com/user-attachments/assets/22c6703c-f334-4720-8c20-e9765036738b" />

##  VLAN 

### Defined VLANs

| VLAN ID | Name     | Purpose      |
|--------|----------|-------------|
| 10     | Seals    | End Devices |
| 20     | IT       | Servers     |
| 30     | Dev      | Management  |

---

### Configuration
Sw1
```
vlan 10
name Seals
exit
vlan 20
name Dev
interface fastethernet f0/1
switchport access vl 10
ex
interface fastethernet f0/2
switchport access vl 20
```
---
SW2
```
Vlan 10
name Seals
exit
vlan 30
name IT
interface fastethernet f0/1
switchport access vl 10
ex
interface fastethernet f0/2
switchport access vl 30
```
---
Sw3
```
vlan 20
name IT
exit
vlan 30
name Dev
interface fastethernet f0/1
switchport access vl 20
ex
interface fastethernet f0/2
switchport access vl 30
```
---
MLS4
```
vlan 10
name Seals
vlan 20
Name Dev
vlan 30
Name IT
```
---
MLS5
```
vlan 10
name Seals
vlan 20
Name Dev
vlan 30
Name IT
```
### Verification
<img width="605" height="219" alt="SW1" src="https://github.com/user-attachments/assets/3901aec8-3d55-4d86-aed2-67c5736574ac" /> 

---
<img width="595" height="221" alt="SW2" src="https://github.com/user-attachments/assets/02f2ad47-9144-4cb8-a194-8bad15729ce3" />

---
<img width="556" height="220" alt="SW3" src="https://github.com/user-attachments/assets/0879f1ba-426e-49e8-b9df-fd12e59a305c" />

---
<img width="771" height="207" alt="MLS4  Vlan" src="https://github.com/user-attachments/assets/355dea62-af46-40de-a235-7ed4755c20e7" />

---
<img width="729" height="223" alt="MLS5 Vlan" src="https://github.com/user-attachments/assets/d5f1dd85-9487-4186-a061-b7ae8735ffa5" />

---

## Trunk

###confingurtion(channelgroup)

Sw1
```
int fastethernet range f0/3,f0/8
switchport mode trunk
switchport trunk allowed vl 10,20
```
---
SW2
```

int fastethernet range f0/4-f0/5
switchport mode trunk
switchport trunk allowed vl 10,30
```
---
Sw3
```

int fastethernet range f0/6-f0/7
switchport mode trunk
switchport trunk allowed vl 20,30
```
---
MLS4
```
int fastethernet range f0/10-f0/11
channel-group 1 mode active
int fastethernet po1
switchport trunk encapsulation dot1q 
switchport mode trunk
ex
write 
```
---
MLS5
```
int fastethernet range f0/10-f0/11
channel-group 1 mode passive
int fastethernet po1
switchport trunk encapsulation dot1q 
switchport mode trunk
ex
write
```
---
### Verification
<img width="501" height="241" alt="SW1Trunk" src="https://github.com/user-attachments/assets/3421883c-075a-4156-862c-a6000134dd19" />

---
<img width="532" height="237" alt="SW2 Trunk" src="https://github.com/user-attachments/assets/96585621-6cdf-4721-bbc2-90de50315010" />

---
<img width="526" height="234" alt="SW3 Trunk" src="https://github.com/user-attachments/assets/d90e4b31-6898-4830-98e5-a27532ee45f8" />

---
<img width="728" height="341" alt="MLS4 Turnk and channel group" src="https://github.com/user-attachments/assets/d5b2cedd-0c3d-4df0-9c16-394025dc793a" />

---
<img width="678" height="334" alt="MLS5 Trunk and channel group" src="https://github.com/user-attachments/assets/82b6d435-77cd-413c-8873-e7460a078ad8" />

---
## SVI

### Configuration
MLS4
```
interface vlan 10
 ip address 192.168.10.126 255.255.255.128

interface vlan 20
 ip address 192.168.20.62 255.255.255.192

interface vlan 30
 ip address 192.168.30.30 255.255.255.224
ex
ip routing 
```
---
MLS5
```
interface vlan 10
 ip address 192.168.10.125 255.255.255.128

interface vlan 20
 ip address 192.168.20.61 255.255.255.192

interface vlan 30
 ip address 192.168.30.29 255.255.255.224
ex
ip routing 
```
---
### Verification

<img width="688" height="102" alt="SVI MLS4" src="https://github.com/user-attachments/assets/bc8033a3-abfc-43bb-b8e6-4dfd448c5433" />

---
<img width="693" height="111" alt="SVI MLS5" src="https://github.com/user-attachments/assets/acf61165-853e-45de-8bff-eebcf129cd47" />

---

## Spanning Tree Protocol (STP)

### Configuration

MLS4

```
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary
spanning-tree vlan 30 root secondary
```
---
MLS5
```
spanning-tree vlan 10 root secondary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
```
## HSRP (Gateway Redundancy)

### Virtual IPs

| VLAN | Virtual IP |
|------|-----------|
| 10   | 192.168.10.124 |
| 20   | 192.168.20.60  |
| 30   | 192.168.30.28  |
---
### configurations
MLS4
```
interface vlan 10
 standby 10 ip 192.168.10.124
 standby 10 priority 110
 standby 10 preempt

interface vlan 20
 standby 20 ip 192.168.20.60
 standby 20 priority 90

interface vlan 30
 standby 30 ip 192.168.30.28
 standby 30 priority 90
 ```
---
MLS5
```
interface vlan 10
 standby 10 ip 192.168.10.124
 standby 10 priority 90

interface vlan 20
 standby 20 ip 192.168.20.60
 standby 20 priority 110
 standby 20 preempt

interface vlan 30
 standby 30 ip 192.168.30.28
 standby 30 priority 110
 standby 30 preempt
 ```
---

### Verification
MLS4
```
Vlan10 - Group 10
  State is Active
    5 state changes, last state change 00:00:27
  Virtual IP address is 192.168.10.124
  Active virtual MAC address is 0000.0C07.AC0A
    Local virtual MAC address is 0000.0C07.AC0A (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 2.9 secs
  Preemption enabled
  Active router is local
  Standby router is 192.168.10.125
  Priority 120 (configured 120)
  Group name is hsrp-Vl1-10 (default)
Vlan20 - Group 20
  State is Standby
    5 state changes, last state change 00:00:29
  Virtual IP address is 192.168.20.60
  Active virtual MAC address is 0000.0C07.AC14
    Local virtual MAC address is 0000.0C07.AC14 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 0.482 secs
  Preemption enabled
  Active router is 192.168.20.61
  Standby router is local
  Priority 80 (configured 80)
  Group name is hsrp-Vl2-20 (default)
Vlan30 - Group 30
  State is Standby
    6 state changes, last state change 00:00:36
  Virtual IP address is 192.168.30.28
  Active virtual MAC address is 0000.0C07.AC1E
    Local virtual MAC address is 0000.0C07.AC1E (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 1.403 secs
  Preemption enabled
  Active router is 192.168.30.29
  Standby router is local
  Priority 80 (configured 80)
  Group name is hsrp-Vl3-30 (default)
```
MLS5
```
Vlan10 - Group 10
  State is Standby
    7 state changes, last state change 00:00:38
  Virtual IP address is 192.168.10.124
  Active virtual MAC address is 0000.0C07.AC0A
    Local virtual MAC address is 0000.0C07.AC0A (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 0.859 secs
  Preemption enabled
  Active router is 192.168.10.126
  Standby router is local
  Priority 100 (default 100)
  Group name is hsrp-Vl1-10 (default)
Vlan20 - Group 20
  State is Active
    4 state changes, last state change 00:00:18
  Virtual IP address is 192.168.20.60
  Active virtual MAC address is 0000.0C07.AC14
    Local virtual MAC address is 0000.0C07.AC14 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 2.077 secs
  Preemption enabled
  Active router is local
  Standby router is 192.168.20.62
  Priority 120 (configured 120)
  Group name is hsrp-Vl2-20 (default)
Vlan30 - Group 30
  State is Active
    5 state changes, last state change 00:00:26
  Virtual IP address is 192.168.30.28
  Active virtual MAC address is 0000.0C07.AC1E
    Local virtual MAC address is 0000.0C07.AC1E (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 0.073 secs
  Preemption enabled
  Active router is local
  Standby router is 192.168.30.30
  Priority 120 (configured 120)
  Group name is hsrp-Vl3-30 (default)
```
---



