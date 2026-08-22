# Security Controls

## Guest Isolation

### Objective

Prevent unmanaged guest endpoints from initiating communication with internal hospital systems.

### Implementation

An extended ACL named `GUEST-ISOLATION` is applied inbound to the Guest VLAN gateway.

### Restricted Destinations

Guest devices are denied access to:

- Clinical
- IoMT
- Administration
- Billing
- Security
- Hospital Servers

Traffic toward non-hospital destinations remains permitted.

### Validation

Guest connectivity tests to internal VLANs failed as expected.

ACL match counters confirmed that the deny statements processed the test traffic.

---

## IoMT Least Privilege

### Objective

Reduce the potential for lateral movement from compromised medical or IoMT devices.

### Implementation

An extended ACL named `IOMT-ISOLATION` is applied inbound to VLAN 20.

### Permitted Communication

IoMT devices are permitted to communicate with:

- Clinical systems
- Approved hospital-server resources

### Restricted Communication

IoMT devices are prevented from initiating connections to:

- Administration
- Billing
- Security

### Validation

The simulated infusion pump was used as the primary validation endpoint.

It successfully reached approved Clinical and Hospital Server resources while communication with Administration, Billing, and Security failed.

---

## VLAN Segmentation

Each major functional area is assigned to a separate VLAN.

This reduces the size of broadcast domains and provides defined Layer 3 control points where ACLs can be applied.

---

## Wireless IoMT Segmentation

Wireless medical devices connect through a dedicated access point associated with VLAN 20.

This keeps simulated medical-device traffic logically separated from Clinical, Administrative, Guest, and Security endpoints.

---

## Perimeter Firewall

ASA1-PERIMETER separates the trusted hospital environment from the simulated Internet.

The firewall uses:

- Inside security level 100
- Outside security level 0
- Static routing
- Global traffic inspection
- Explicit access control required by the Packet Tracer ASA simulation

---

## NAT/PAT

R1-HOSPITAL performs PAT for internal hospital networks before traffic crosses the perimeter.

Internal RFC1918 addresses are translated to the R1-to-ASA transit address before reaching the ASA.

This design was used to accommodate Packet Tracer ASA simulation behavior.

---

## Inbound Protection

The simulated public server is unable to directly reach internal hospital resources such as `HOSPITAL-SRV01`.

This demonstrates that Internet-originated traffic does not receive unrestricted access to the hospital network.
