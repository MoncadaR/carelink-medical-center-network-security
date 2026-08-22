# Troubleshooting Notes

## ASA Return-Path Failure

### Symptom

Hospital clients could send traffic toward the simulated public server, but replies did not successfully return through the perimeter.

### Initial Checks

The following components were verified independently:

- VLAN configuration
- Router-on-a-Stick
- DHCP
- inter-VLAN routing
- R1 default route
- ASA inside interface
- ASA outside interface
- ASA static routes
- ISP routing
- public-server IP configuration
- public HTTP service

### NAT Verification

NAT/PAT counters and translations confirmed that internal traffic was being translated.

This ruled out a simple source-NAT configuration failure.

### Simulation Mode Investigation

Cisco Packet Tracer Simulation Mode was used to follow packets hop-by-hop.

The outbound packet successfully followed:

`R1-HOSPITAL → ASA1-PERIMETER → ISP-R1 → PUBLIC-WEB-SRV`

The return packet followed:

`PUBLIC-WEB-SRV → ISP-R1 → ASA1-PERIMETER`

and was dropped at the ASA.

### Root Cause

The Packet Tracer ASA simulation treated the returning traffic entering the lower-security `outside` interface and leaving the higher-security `inside` interface as requiring an explicit extended ACL permit.

### Resolution

A narrowly scoped outside ACL was introduced for the required simulated return flow.

R1-HOSPITAL was used for PAT to simplify the Packet Tracer forwarding path.

### Lessons Learned

The troubleshooting process reinforced several principles:

1. Verify each network layer independently.
2. Avoid changing multiple controls simultaneously.
3. Use packet-level simulation to identify the exact failure point.
4. Validate both forward and return paths.
5. Treat simulator-specific behavior separately from real-device behavior.
