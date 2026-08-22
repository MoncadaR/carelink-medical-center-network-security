<div align="center">

# CareLink Medical Center
## Network Architecture

**Logical design, segmentation model, addressing, and traffic flow**

<p>
  <img src="https://img.shields.io/badge/Architecture-Hospital%20Network-0A7EA4?style=for-the-badge">
  <img src="https://img.shields.io/badge/VLANs-7%20Security%20Zones-2F855A?style=for-the-badge">
  <img src="https://img.shields.io/badge/Routing-Router--on--a--Stick-5A67D8?style=for-the-badge">
</p>

**[← Back to README](../README.md) · [Security Controls](security-controls.md) · [Threat Model](threat-model.md) · [Validation](test-matrix.md)**

</div>

---

## Architecture Overview

CareLink Medical Center is a simulated hospital network built to demonstrate how network segmentation, routing, wireless IoMT connectivity, and perimeter security can be combined into a healthcare-focused architecture.

The design separates systems by both **business function** and **security trust level**.

```mermaid
flowchart TB

    subgraph CARELINK["CareLink Medical Center"]

        subgraph ENDPOINTS["Hospital Security Zones"]
            direction LR
            V10["VLAN 10<br/>Clinical"]
            V20["VLAN 20<br/>IoMT"]
            V30["VLAN 30<br/>Administration"]
            V40["VLAN 40<br/>Billing"]
            V50["VLAN 50<br/>Security"]
            V60["VLAN 60<br/>Guest"]
            V70["VLAN 70<br/>Hospital Servers"]
        end

        AP["AP-IOMT01<br/>Wireless Medical Devices"]
        SW["SW1-HOSPITAL<br/>Layer 2 Switching"]
        R1["R1-HOSPITAL<br/>Inter-VLAN Routing<br/>DHCP · ACLs · PAT"]
        ASA["ASA1-PERIMETER<br/>Hospital Firewall"]

        AP --> V20

        V10 --> SW
        V20 --> SW
        V30 --> SW
        V40 --> SW
        V50 --> SW
        V60 --> SW
        V70 --> SW

        SW == "802.1Q Trunk" ==> R1
        R1 --> ASA
    end

    ISP["ISP-R1<br/>Simulated ISP"]
    WEB["PUBLIC-WEB-SRV<br/>198.51.100.10"]

    ASA --> ISP
    ISP --> WEB
```

---

## Architecture at a Glance

| Layer | Component | Role |
|---|---|---|
| Access | `SW1-HOSPITAL` | VLAN segmentation and endpoint connectivity |
| Wireless | `AP-IOMT01` | Wireless IoMT access |
| Routing | `R1-HOSPITAL` | Inter-VLAN routing, DHCP, ACL enforcement, PAT |
| Perimeter | `ASA1-PERIMETER` | Inside/outside security boundary |
| External | `ISP-R1` | Simulated Internet routing |
| External Service | `PUBLIC-WEB-SRV` | Simulated public application |
| Internal Service | `HOSPITAL-SRV01` | Simulated hospital application server |

---

## Network Segmentation

| VLAN | Security Zone | Network | Gateway | Example Systems |
|---:|---|---|---|---|
| 10 | Clinical | `192.168.10.0/24` | `192.168.10.1` | Nurse / Doctor workstations |
| 20 | IoMT | `192.168.20.0/24` | `192.168.20.1` | Infusion pump / patient monitors |
| 30 | Administration | `192.168.30.0/24` | `192.168.30.1` | Administrative workstation |
| 40 | Billing | `192.168.40.0/24` | `192.168.40.1` | Billing workstation |
| 50 | Security | `192.168.50.0/24` | `192.168.50.1` | SOC workstation |
| 60 | Guest | `192.168.60.0/24` | `192.168.60.1` | Guest laptop |
| 70 | Hospital Servers | `192.168.70.0/24` | `192.168.70.1` | Internal hospital server |

---

## IoMT Architecture

The medical-device environment contains both wired and wireless systems.

```mermaid
flowchart LR
    PUMP["INFUSION-PUMP01<br/>192.168.20.21"]
    SIG1["SIGNAL-MONITOR01<br/>192.168.20.22"]
    SIG2["SIGNAL-MONITOR02<br/>192.168.20.23"]
    PATIENT["PATIENT-MONITOR01<br/>192.168.20.24"]

    AP["AP-IOMT01"]
    SW["SW1-HOSPITAL<br/>VLAN 20"]

    PUMP -. Wi-Fi .-> AP
    SIG1 -. Wi-Fi .-> AP
    SIG2 -. Wi-Fi .-> AP
    AP --> SW
    PATIENT --> SW
```

Wireless devices associate with `AP-IOMT01`, which bridges them into VLAN 20.

This gives the lab a dedicated IoMT trust zone rather than placing simulated medical devices directly alongside Clinical or Administrative systems.

---

## Router-on-a-Stick Design

`SW1-HOSPITAL` carries all internal VLANs to `R1-HOSPITAL` over a single 802.1Q trunk.

```mermaid
flowchart LR
    SW["SW1-HOSPITAL<br/>Gi0/1"]
    TRUNK["802.1Q Trunk"]
    ROUTER["R1-HOSPITAL<br/>Gi0/0"]

    SW ==> TRUNK ==> ROUTER

    ROUTER --> V10["Gi0/0.10<br/>192.168.10.1"]
    ROUTER --> V20["Gi0/0.20<br/>192.168.20.1"]
    ROUTER --> V30["Gi0/0.30<br/>192.168.30.1"]
    ROUTER --> V40["Gi0/0.40<br/>192.168.40.1"]
    ROUTER --> V50["Gi0/0.50<br/>192.168.50.1"]
    ROUTER --> V60["Gi0/0.60<br/>192.168.60.1"]
    ROUTER --> V70["Gi0/0.70<br/>192.168.70.1"]
```

---

## Addressing Strategy

Dynamic addressing is provided by `R1-HOSPITAL`.

Addresses `.1` through `.20` are reserved within client VLANs for infrastructure or future static assignments.

Example:

```text
192.168.10.1      Default gateway
192.168.10.2-20   Reserved
192.168.10.21+    DHCP clients
```

The internal hospital server uses a static address:

```text
HOSPITAL-SRV01
192.168.70.10/24
Gateway: 192.168.70.1
```

---

## Perimeter Architecture

The hospital network connects to the simulated Internet through a dedicated transit network and ASA firewall.

```mermaid
flowchart LR
    R1["R1-HOSPITAL<br/>10.0.0.1"]
    ASA_IN["ASA Inside<br/>10.0.0.2"]
    ASA_OUT["ASA Outside<br/>203.0.113.2"]
    ISP["ISP-R1<br/>203.0.113.1"]
    WEB["PUBLIC-WEB-SRV<br/>198.51.100.10"]

    R1 --> ASA_IN
    ASA_IN --> ASA_OUT
    ASA_OUT --> ISP
    ISP --> WEB
```

### External Networks

| Purpose | Network |
|---|---|
| R1 ↔ ASA transit | `10.0.0.0/30` |
| ASA ↔ ISP transit | `203.0.113.0/30` |
| Simulated Internet | `198.51.100.0/24` |

---

## Traffic Flow

### Internal

```text
Endpoint
  ↓
SW1-HOSPITAL
  ↓
802.1Q Trunk
  ↓
R1-HOSPITAL
  ↓
Destination VLAN
```

### External

```text
Hospital Endpoint
  ↓
SW1-HOSPITAL
  ↓
R1-HOSPITAL
  ↓
PAT
  ↓
ASA1-PERIMETER
  ↓
ISP-R1
  ↓
PUBLIC-WEB-SRV
```

---

## Design Decisions

| Decision | Reason |
|---|---|
| Separate IoMT VLAN | Reduce medical-device lateral movement |
| Separate Guest VLAN | Isolate unmanaged systems |
| Dedicated Security VLAN | Preserve administrative/monitoring reachability |
| Static Hospital Server | Stable address for policy and testing |
| Router-on-a-Stick | Efficient inter-VLAN routing in Packet Tracer |
| ASA perimeter | Demonstrate firewall trust boundaries |
| PAT | Hide internal addressing and enable simulated Internet access |

---

<details>
<summary><strong>View Packet Tracer implementation</strong></summary>

<br>

![CareLink Logical Topology](../diagrams/carelink-logical-topology.png)

</details>

---

## Scope

This architecture is intentionally simplified for educational purposes.

It demonstrates network-security engineering concepts but is not intended to represent a production hospital network, clinical safety design, or compliance-certified healthcare architecture.

---

<div align="center">

**[← Back to Project Overview](../README.md)**

</div>
