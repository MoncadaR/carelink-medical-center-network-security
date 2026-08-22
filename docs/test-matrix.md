# Security Validation Matrix

| ID | Source | Destination | Expected | Result |
|---|---|---|---|---|
| T01 | Clinical | Hospital Server | Allow | PASS |
| T02 | Guest | Clinical | Deny | PASS |
| T03 | Guest | IoMT | Deny | PASS |
| T04 | Guest | Administration | Deny | PASS |
| T05 | Guest | Billing | Deny | PASS |
| T06 | Guest | Security | Deny | PASS |
| T07 | Guest | Hospital Server | Deny | PASS |
| T08 | IoMT | Administration | Deny | PASS |
| T09 | IoMT | Billing | Deny | PASS |
| T10 | IoMT | Security | Deny | PASS |
| T11 | IoMT | Clinical | Allow | PASS |
| T12 | IoMT | Hospital Server | Allow | PASS |
| T13 | Security | IoMT | Allow | PASS |
| T14 | Clinical | Public Web Server | Allow | PASS |
| T15 | Security | Public Web Server | Allow | PASS |
| T16 | Public Web Server | Hospital Server | Deny | PASS |

## Validation Methods

Testing included:

- ICMP connectivity
- HTTP connectivity
- ACL match counters
- DHCP binding verification
- VLAN verification
- trunk verification
- route inspection
- NAT/PAT translation inspection
- Packet Tracer Simulation Mode
