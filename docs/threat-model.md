# Threat Model

## Purpose

This threat model identifies several representative security concerns for the simulated CareLink Medical Center network.

## Protected Assets

- Clinical workstations
- Medical and IoMT devices
- Administrative systems
- Billing systems
- Hospital application servers
- Security management workstations
- Network infrastructure

## Threat Scenarios

### Compromised IoMT Device

A vulnerable medical device could be used as an entry point for lateral movement.

**Mitigation:**
- Dedicated IoMT VLAN
- ACL-based least privilege
- Restricted communication with Administration, Billing, and Security

### Unauthorized Guest Access

An unmanaged guest endpoint could attempt to access internal hospital systems.

**Mitigation:**
- Dedicated Guest VLAN
- Guest isolation ACL
- No direct access to internal hospital zones

### Ransomware Lateral Movement

A compromised workstation could attempt to spread across unrelated network segments.

**Mitigation:**
- VLAN segmentation
- Inter-VLAN filtering
- Separation of IoMT, Clinical, Billing, Security, and Guest systems

### Internet-Originated Access

External systems could attempt to initiate connections toward hospital resources.

**Mitigation:**
- ASA perimeter firewall
- No direct publication of internal hospital resources
- Controlled routing and access policy

### Misconfiguration

Incorrect VLAN assignments, ACLs, routes, NAT rules, or gateways could cause outages or excessive access.

**Mitigation:**
- Incremental configuration
- Positive and negative controls
- ACL match counters
- Route verification
- Packet Tracer Simulation Mode

## Trust Boundaries

The major trust boundaries are:

1. Guest network vs internal hospital networks
2. IoMT network vs business/administrative systems
3. Internal hospital network vs simulated Internet
4. Security management network vs general endpoints

## Limitations

This is an educational threat model.

It does not model every healthcare threat, clinical safety consideration, regulatory requirement, or production security control.
