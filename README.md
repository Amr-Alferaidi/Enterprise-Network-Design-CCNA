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
switchport trunk allowed vl 10
switchport trunk allowed vl 20
```
---
SW2
```

int fastethernet range f0/4-f0/5
switchport mode trunk
switchport trunk allowed vl 10
switchport trunk allowed vl 30
```
---
Sw3
```

int fastethernet range f0/6-f0/7
switchport mode trunk
switchport trunk allowed vl 20
switchport trunk allowed vl 30
```
---
MLS4
```
int fastethernet range f0/10-f0/11
channel-group 1 mode active
int fastethernet po1
switchport trunk encapsulation dot1q 
switchport mode trunk
```
---
MLS5
```
int fastethernet range f0/10-f0/11
channel-group 1 mode passive
int fastethernet po1
switchport trunk encapsulation dot1q 
switchport mode trunk
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




