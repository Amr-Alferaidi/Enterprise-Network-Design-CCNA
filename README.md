# Enterprise-Network-Design-CCNA

## Devices
2 Core routers,2 dis MLSs ,3 access SWs,6 end Node

## Topology

<img width="1366" height="673" alt="Topology" src="https://github.com/user-attachments/assets/22c6703c-f334-4720-8c20-e9765036738b" />

## 🟢 VLAN Configuration

### Defined VLANs

| VLAN ID | Name     | Purpose      |
|--------|----------|-------------|
| 10     | Seals    | End Devices |
| 20     | IT       | Servers     |
| 30     | Dev      | Management  |

---

### Configuration (Access)
Sw1

vlan 10
name Seals

vlan 20
name IT

---
SW2

Vlan 10
name Seals

vlan 30
name Dev

---
Sw3

vlan 20
seals

vlan 30
Dev

### Verification
<img width="605" height="219" alt="SW1" src="https://github.com/user-attachments/assets/3901aec8-3d55-4d86-aed2-67c5736574ac" />
<img width="595" height="221" alt="SW2" src="https://github.com/user-attachments/assets/02f2ad47-9144-4cb8-a194-8bad15729ce3" />
<img width="556" height="220" alt="SW3" src="https://github.com/user-attachments/assets/0879f1ba-426e-49e8-b9df-fd12e59a305c" />


