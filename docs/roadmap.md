# Project status and roadmap

## Last known status

The review presentation describes the architecture and security review as complete. The project owner reported that the Proxmox project drive became corrupted and the guest VMs are unavailable. This repository was empty when documentation work began; it contains no implementation, VM definitions, configurations, test captures, or verification results yet.

All IP values in [the network plan](network-plan.md) are proposed and simulated. Do not present them as recovered or tested guest addresses.

## Planned phases

1. **Restore and harden the hypervisor:** recover Proxmox storage, create internal-only segmented networks, and document the firewall policy.
2. **Rebuild the decoy:** restore Cowrie and validate its logging behavior with controlled SSH tests.
3. **Establish telemetry:** forward Cowrie events to Splunk and record a baseline from authorized lab activity.
4. **Add analysis:** implement and evaluate the Python classifier in observe-only mode; document input data, model, weights, threshold, and false-positive results.
5. **Add guarded response:** integrate FastAPI SOAR and the firewall only after authentication, allowlisting, rule expiry, auditing, and rollback are verified.
6. **Evaluate deception upgrades:** assess the proposed MITRE Engage activities and any dynamic shell or ephemeral target work only within the isolated lab.

## Completion evidence

For each phase, add the configuration or source file, a short test procedure, the observed result, and sanitized evidence. Keep planned, implemented, and verified status separate in future updates.
