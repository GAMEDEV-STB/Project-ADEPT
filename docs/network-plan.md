# Isolated lab network plan

## Status

These are **proposed, simulated addresses** for rebuilding the lab. They are not recovered addresses and have not been confirmed on running VMs. The host `ipconfig` output identifies host adapters, including Wi-Fi at `192.168.1.17`; it does not identify the Proxmox guest addresses.

The `10.77.0.0/16` private range is separate from the shown Wi-Fi subnet (`192.168.1.0/24`) and WSL adapter range (`172.24.80.0/20`). Recheck for conflicts on the actual Proxmox host before assigning it.

## Proposed addressing

| Segment | VM or interface | Proposed address | Purpose |
| --- | --- | --- | --- |
| Attack | Kali attacker VM | `10.77.10.10/24` | Authorized test traffic |
| Attack | Virtual firewall | `10.77.10.1/24` | Segment gateway, if routing is required |
| Decoy/DMZ | Cowrie | `10.77.20.10/24` | SSH honeypot |
| Decoy/DMZ | Virtual firewall | `10.77.20.1/24` | Segment gateway |
| Telemetry | Splunk SIEM | `10.77.30.10/24` | Log collection and search |
| Telemetry | Virtual firewall | `10.77.30.1/24` | Segment gateway |
| Analysis | Python classifier | `10.77.40.10/24` | Event scoring |
| Analysis | Virtual firewall | `10.77.40.1/24` | Segment gateway |
| Containment | FastAPI SOAR | `10.77.50.10/24` | Validated response requests |
| Containment | Virtual firewall | `10.77.50.1/24` | Segment gateway |

The `.1` values are part of the proposed plan; they only apply if a virtual firewall is configured with those interfaces. Do not configure a default route to the physical LAN or enable NAT for this lab.

## Allowed paths to configure

| Source | Destination | Planned traffic | Notes |
| --- | --- | --- | --- |
| Kali | Cowrie | SSH, TCP/22 | Primary controlled test path |
| Cowrie | Splunk | Selected log forwarding | Select and document the Splunk input protocol and port during deployment |
| Splunk/classifier | Classifier | Event data for scoring | Prefer a bounded, authenticated input; implementation is TBD |
| Classifier | SOAR | Decision request | Authenticated API; endpoint and port are TBD |
| SOAR | Virtual firewall | Temporary rule update | Allowlist permitted actions; require expiry and audit logging |

Apply a default-deny policy between segments and deny outbound Internet access. Add only the paths required for the test. Do not expose Cowrie to a public address as part of this project plan.

## Rebuild checks

After restoring the VMs, verify each guest address and route from inside that guest. Confirm the firewall interfaces, allowed routes, and absence of WAN/NAT connectivity. Test only the listed paths, then confirm that blocked paths stay blocked. Record the observed results and replace this proposal only with verified configuration evidence.
