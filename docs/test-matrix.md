<div align="center">

# CareLink Medical Center
## Security Validation Matrix

**Positive controls, negative controls, and evidence-based verification**

<p>
  <img src="https://img.shields.io/badge/Tests-16-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Result-PASS-success?style=for-the-badge">
  <img src="https://img.shields.io/badge/Method-Positive%20%2B%20Negative%20Controls-5A67D8?style=for-the-badge">
</p>

**[← README](../README.md) · [Architecture](architecture.md) · [Security Controls](security-controls.md) · [Troubleshooting](troubleshooting.md)**

</div>

---

## Validation Strategy

The network was tested using both:

**Positive controls** — traffic that should succeed.

**Negative controls** — traffic that should be denied.

This avoids treating connectivity alone as proof of correct security behavior.

---

## Overall Result

```mermaid
pie showData
    title CareLink Security Validation
    "Passed" : 16
    "Failed" : 0
```

---

## Test Matrix

| ID | Category | Source | Destination | Expected | Result |
|---|---|---|---|---|:---:|
| T01 | Clinical | Clinical | Hospital Server | Allow |  PASS |
| T02 | Guest | Guest | Clinical | Deny | PASS |
| T03 | Guest | Guest | IoMT | Deny |  PASS |
| T04 | Guest | Guest | Administration | Deny |  PASS |
| T05 | Guest | Guest | Billing | Deny |  PASS |
| T06 | Guest | Guest | Security | Deny |  PASS |
| T07 | Guest | Guest | Hospital Server | Deny |  PASS |
| T08 | IoMT | IoMT | Administration | Deny |  PASS |
| T09 | IoMT | IoMT | Billing | Deny |  PASS |
| T10 | IoMT | IoMT | Security | Deny |  PASS |
| T11 | IoMT | IoMT | Clinical | Allow |  PASS |
| T12 | IoMT | IoMT | Hospital Server | Allow |  PASS |
| T13 | Security | SOC | IoMT | Allow | PASS |
| T14 | Perimeter | Clinical | Public Web Server | Allow | PASS |
| T15 | Perimeter | Security | Public Web Server | Allow | PASS |
| T16 | Perimeter | Public Web Server | Hospital Server | Deny |  PASS |

---

## Guest Isolation Validation

```mermaid
flowchart LR
    G["Guest<br/>192.168.60.21"]

    G -- FAIL --> C["Clinical"]
    G -- FAIL --> I["IoMT"]
    G -- FAIL --> A["Administration"]
    G -- FAIL --> B["Billing"]
    G -- FAIL --> S["Security"]
    G -- FAIL --> H["Hospital Server"]
```

Result:

```text
6/6 expected denies passed
```

<details>
<summary><strong>Evidence</strong></summary>

<br>

![Guest Isolation](../screenshots/05-guest-isolation.png)

</details>

---

## IoMT Validation

Primary test endpoint:

```text
INFUSION-PUMP01
192.168.20.21
```

| Destination | Before ACL | After ACL |
|---|:---:|:---:|
| Administration | ✅ | ❌ |
| Billing | ✅ | ❌ |
| Security | ✅ | ❌ |
| Clinical | ✅ | ✅ |
| Hospital Server | ✅ | ✅ |

This demonstrates an actual **security-state change**, not just static configuration.

<details>
<summary><strong>Evidence</strong></summary>

<br>

![IoMT Isolation](../screenshots/06-iomt-isolation.png)

</details>

---

## Perimeter Validation

```mermaid
flowchart LR
    H["Hospital"]
    FW["ASA"]
    W["Public Web Server"]

    H -- "PASS" --> FW
    FW -- "PASS" --> W

    W -- "BLOCKED unsolicited" --> FW
    FW -. DENY .-> H
```

Validated outcomes:

```text
Hospital → Internet      PASS
Internet → Hospital      DENY
```

---

## Verification Methods

| Method | Purpose |
|---|---|
| `ping` | Layer 3 reachability |
| HTTP | Application connectivity |
| `show vlan brief` | VLAN membership |
| `show interfaces trunk` | Trunk verification |
| `show ip dhcp binding` | DHCP verification |
| `show access-lists` | ACL policy and counters |
| `show ip route` | Routing validation |
| `show ip nat translations` | PAT validation |
| Packet Tracer Simulation Mode | Hop-by-hop packet analysis |

---

## Evidence Gallery

<details>
<summary><strong>VLANs</strong></summary>

<br>

![VLANs](../screenshots/02-vlan-segmentation.png)

</details>

<details>
<summary><strong>802.1Q Trunk</strong></summary>

<br>

![Trunk](../screenshots/03-dot1q-trunk.png)

</details>

<details>
<summary><strong>DHCP</strong></summary>

<br>

![DHCP](../screenshots/04-dhcp-bindings.png)

</details>

<details>
<summary><strong>NAT/PAT</strong></summary>

<br>

![NAT](../screenshots/08-nat-pat.png)

</details>

<details>
<summary><strong>Outbound Web Access</strong></summary>

<br>

![Web](../screenshots/09-outbound-web-access.png)

</details>

---

## Final Validation Status

> **All documented security tests produced the expected result.**

---

<div align="center">

**[← Back to Project Overview](../README.md)**

</div>
