# Rebuild and verification checklist

This is a plan for restoring the lab after the reported Proxmox project-drive failure. No commands or package versions are assumed because the repository does not yet contain VM definitions or deployment configuration.

## Before starting

- Restore Proxmox storage and confirm the host is stable.
- Take a backup or snapshot before making changes.
- Use an internal-only virtual bridge with no physical uplink.
- Keep the lab off the campus network and the public Internet.
- Use disposable VMs and synthetic credentials.

## Rebuild sequence

1. Create the isolated virtual networks described in the [network plan](network-plan.md).
2. Configure the virtual firewall with one interface per subnet, default-deny inter-zone rules, and no WAN/NAT path.
3. Rebuild the Kali and Cowrie VMs; assign the proposed addresses only after checking for conflicts.
4. Rebuild Splunk and configure a dedicated, restricted log input for Cowrie.
5. Add the Python classifier in observe-only mode. Start with saved or synthetic events before connecting it to live lab telemetry.
6. Add the SOAR API only after authentication, input validation, and an allowlist of permitted firewall actions are in place.
7. Test automatic rule expiry and a manual rollback before enabling any enforcement.

## Verification checklist

- [ ] Each guest reports the intended address, prefix, and route.
- [ ] The lab has no route to the campus network or public Internet.
- [ ] Kali can reach Cowrie only on the intended SSH test path.
- [ ] Cowrie events arrive in Splunk with timestamps and source addresses intact.
- [ ] The classifier logs a decision for known test events without changing firewall state.
- [ ] Benign and test events are reviewed for false positives before setting a threshold.
- [ ] SOAR rejects unauthenticated, malformed, expired, or out-of-scope requests.
- [ ] A test block expires on schedule and is recorded in an audit log.
- [ ] A tested rollback restores the expected firewall policy.

Record the date, VM/software versions, expected result, observed result, and evidence for each completed check. Mark an item complete only after the check has actually passed.
