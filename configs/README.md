# Device Configurations

These text files capture the relevant configuration for each simulated network device.
They make the implementation reviewable without requiring Cisco Packet Tracer.

| Device | Role | Configuration |
|---|---|---|
| `SW1-HOSPITAL` | VLAN creation, access ports, and 802.1Q trunking | [View](SW1-HOSPITAL.txt) |
| `R1-HOSPITAL` | Inter-VLAN routing, DHCP, ACLs, static routing, and PAT | [View](R1-HOSPITAL.txt) |
| `ASA1-PERIMETER` | Inside/outside boundary, routing, inspection, and return-flow control | [View](ASA1-PERIMETER.txt) |
| `ISP-R1` | Simulated external routing | [View](ISP-R1.txt) |

## Suggested Review Order

1. Confirm the VLANs and trunk on `SW1-HOSPITAL`.
2. Trace each VLAN gateway to the router subinterfaces on `R1-HOSPITAL`.
3. Review `GUEST-ISOLATION` and the IoMT policy before checking NAT/PAT.
4. Follow the default route from the hospital router through the ASA and simulated ISP.
5. Compare the policy with the [validation matrix](../docs/test-matrix.md).

## Safety and Scope

The configurations belong to a fictional Packet Tracer environment. They contain no
production addresses, credentials, patient information, or real hospital details. They
are educational examples, not vendor hardening guidance or a production baseline.

[← Back to project overview](../README.md)
