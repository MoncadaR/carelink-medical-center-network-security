<div align="center">

# CareLink Medical Center
## NIST Cybersecurity Framework 2.0 Mapping

**A portfolio-level view of demonstrated cybersecurity outcomes and intentional gaps**

![Framework](https://img.shields.io/badge/NIST-CSF%202.0-183A61?style=for-the-badge)
![Scope](https://img.shields.io/badge/Scope-Educational%20Mapping-5A67D8?style=for-the-badge)
![Compliance](https://img.shields.io/badge/Compliance-Not%20Claimed-C05621?style=for-the-badge)

**[← README](../README.md) · [Architecture](architecture.md) · [Security Controls](security-controls.md) · [Threat Model](threat-model.md) · [Validation](test-matrix.md)**

</div>

---

## Purpose

NIST CSF 2.0 organizes cybersecurity outcomes into six Functions: **Govern,
Identify, Protect, Detect, Respond, and Recover**. This document shows where the
CareLink lab provides evidence related to those outcomes and, just as importantly,
where the simulation has gaps.

This is an interpretive educational mapping. It is **not** a NIST assessment,
organizational profile, maturity rating, HIPAA evaluation, or compliance claim.

Official reference: [NIST Cybersecurity Framework 2.0](https://doi.org/10.6028/NIST.CSWP.29)

---

## Function-Level Mapping

| CSF 2.0 Function | CareLink Evidence | Coverage |
|---|---|:---:|
| **Govern (GV)** | Documented security objectives, assumptions, scope, risk rationale, and limitations | Partial |
| **Identify (ID)** | Asset groups, network zones, trust boundaries, threat scenarios, and qualitative risk matrix | Demonstrated |
| **Protect (PR)** | VLAN segmentation, ACL-based least privilege, perimeter firewalling, NAT/PAT, and management separation | Demonstrated |
| **Detect (DE)** | ACL/NAT counters, verification commands, and Packet Tracer Simulation Mode | Partial |
| **Respond (RS)** | Structured investigation and mitigation of the ASA return-path issue | Partial |
| **Recover (RC)** | No recovery plan, backup workflow, restoration test, or clinical continuity exercise | Not implemented |

---

## Evidence Traceability

| Demonstrated Outcome | Implementation | Evidence |
|---|---|---|
| Systems are grouped by role and trust | Seven VLAN security zones | [Architecture](architecture.md) |
| Network access is limited according to policy | Guest and IoMT ACLs | [Security controls](security-controls.md) |
| External and internal boundaries are defined | ASA inside/outside perimeter | [Architecture](architecture.md) |
| Protective controls are verified | Positive and negative test cases | [Validation matrix](test-matrix.md) |
| Network activity can be inspected | ACL counters, NAT counters, and Simulation Mode | [Validation methods](test-matrix.md#verification-methods) |
| Technical issues are analyzed methodically | Forward/return-path investigation | [Troubleshooting case study](troubleshooting.md) |
| Risks and residual exposure are communicated | Scenario-based threat analysis | [Threat model](threat-model.md) |

---

## Current-to-Target View

| Area | Current Lab State | Target Production Capability |
|---|---|---|
| Asset management | Logical device and zone inventory | Authoritative inventory with ownership, criticality, and lifecycle status |
| Access control | Network ACLs and trust zones | Identity-aware NAC, 802.1X, privileged access management, and protocol-specific rules |
| Monitoring | Manual counters and packet simulation | Centralized logs, SIEM correlation, NDR, alerting, and time synchronization |
| Vulnerability management | Threat scenarios only | Authenticated scanning, SBOM/VEX, remediation SLAs, and exception handling |
| Incident response | One technical troubleshooting case | Exercised incident-response plan with clinical, legal, privacy, and vendor roles |
| Recovery | Not implemented | Tested backups, configuration recovery, downtime procedures, and resilience exercises |
| Governance | Educational scope and risk rationale | Approved policies, assigned accountability, risk tolerance, and third-party oversight |

---

## Next Security Iterations

1. Add centralized syslog and alert-use-case documentation.
2. Introduce a simulated network-access-control decision point.
3. Restrict approved IoMT flows by destination and protocol rather than subnet alone.
4. Add an asset register with owner, criticality, software version, and lifecycle status.
5. Create an incident-response scenario for a compromised infusion pump.
6. Document and test configuration backup and recovery.

---

## Interpretation Note

The NIST CSF describes outcomes rather than prescribing one implementation. A control
shown in this lab can contribute to an outcome without satisfying the full outcome in
a real organization. Production healthcare environments also require clinical-safety,
privacy, regulatory, operational-resilience, vendor-management, and human-process
considerations beyond this network simulation.

<div align="center">

**[← Back to Project Overview](../README.md)**

</div>
