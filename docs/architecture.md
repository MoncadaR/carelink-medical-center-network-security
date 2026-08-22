# Architecture

## Overview

CareLink Medical Center is a simulated healthcare network designed to demonstrate secure segmentation, IoMT connectivity, controlled inter-VLAN routing, and perimeter security.

The design separates clinical, medical-device, administrative, financial, security, guest, and server systems into dedicated network zones.

## Network Zones

| VLAN | Zone | Network | Gateway |
|---:|---|---|---|
| 10 | Clinical | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IoMT | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Administration | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Billing | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Security | 192.168.50.0/24 | 192.168.50.1 |
| 60 | Guest | 192.168.60.0/24 | 192.168.60.1 |
| 70 | Hospital Servers | 192.168.70.0/24 | 192.168.70.1 |

## Core Components

- **SW1-HOSPITAL** - Layer 2 switching and VLAN segmentation
- **R1-HOSPITAL** - Router-on-a-Stick, DHCP, ACLs, routing, and PAT
- **AP-IOMT01** - Wireless connectivity for simulated medical devices
- **ASA1-PERIMETER** - Perimeter firewall and security boundary
- **ISP-R1** - Simulated Internet service provider
- **HOSPITAL-SRV01** - Simulated internal hospital application server
- **PUBLIC-WEB-SRV** - Simulated public Internet web service

## Internal Traffic Flow

Hospital endpoints connect to SW1-HOSPITAL through access ports assigned to dedicated VLANs.

SW1-HOSPITAL carries VLAN traffic to R1-HOSPITAL over an 802.1Q trunk.

R1-HOSPITAL provides Layer 3 gateways for each VLAN and performs inter-VLAN routing.

Extended access-control lists restrict communication between selected security zones.

## IoMT Connectivity

The IoMT network includes both wired and wireless devices.

Wireless medical devices connect to AP-IOMT01, which bridges them into VLAN 20.

Example IoMT endpoints include:

- Infusion Pump
- Patient Monitor
- Signal Monitor 01
- Signal Monitor 02

## Perimeter Traffic Flow

External-bound traffic follows this path:

Clinical / IoMT / Administrative / Security Networks
→ SW1-HOSPITAL
→ R1-HOSPITAL
→ ASA1-PERIMETER
→ ISP-R1
→ PUBLIC-WEB-SRV

## External Addressing

| Segment | Network |
|---|---|
| R1-to-ASA transit | 10.0.0.0/30 |
| ASA-to-ISP transit | 203.0.113.0/30 |
| Simulated public network | 198.51.100.0/24 |

Important addresses:

- R1-HOSPITAL outside-facing interface: `10.0.0.1`
- ASA inside: `10.0.0.2`
- ASA outside: `203.0.113.2`
- ISP-R1 ASA-facing interface: `203.0.113.1`
- ISP-R1 public-network gateway: `198.51.100.1`
- PUBLIC-WEB-SRV: `198.51.100.10`
- HOSPITAL-SRV01: `192.168.70.10`

## Design Scope

This architecture is intentionally simplified for Cisco Packet Tracer.

It demonstrates network-security concepts rather than representing a production hospital design.
