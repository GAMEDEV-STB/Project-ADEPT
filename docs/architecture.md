# Architecture and data flow

## Purpose

ADEPT is designed to move a small defensive lab from collecting logs toward controlled analysis and response. Its intended test case is SSH activity sent to a decoy service in a private, segmented Proxmox environment.

The components and flow below describe the project plan. They do not claim that the VMs, integrations, classifier, or automated response are currently running.

## Components

| Component | Planned role | Boundary |
| --- | --- | --- |
| Kali attacker VM | Generate authorized test SSH activity | Attack subnet; lab use only |
| Cowrie | Present an SSH decoy and record interaction | Decoy/DMZ subnet; no trusted credentials |
| Splunk SIEM | Store and search honeypot telemetry | Telemetry subnet; restricted ingest and administration |
| Python classifier | Score selected events for suspicious behavior | Analysis subnet; read-only input where practical |
| FastAPI SOAR controller | Receive a validated decision and request a temporary firewall action | Containment subnet; authenticated, allowlisted API |
| Virtual firewall | Enforce inter-subnet policy and temporary blocks | One interface per lab subnet; no WAN/NAT in the proposed plan |

## Planned event flow

1. The Kali VM connects to Cowrie over SSH on TCP port 22.
2. Cowrie records the interaction and forwards selected logs to Splunk. The precise Splunk input protocol and port remain to be chosen during deployment.
3. The classifier evaluates selected events. Its input format, model, weights, and threshold remain to be implemented and measured.
4. A high-confidence decision may be sent to the SOAR controller through an authenticated API. The API endpoint and port remain to be configured.
5. SOAR may request a short-lived firewall rule for the source address. The rule should expire automatically and leave an audit record.

Every path should be explicitly allowed by the virtual firewall. Default-deny rules should block other inter-zone traffic and all Internet egress.

## Detection design

The review deck proposes combining authentication velocity, critical command activity, threat-intelligence context, and an ML anomaly score. Normalize the inputs before applying weights. Do not choose weights or a threshold until representative benign and test events have been collected and evaluated.

The initial integration should run in **observe-only mode**: record proposed decisions without changing firewall policy. Enable temporary blocking only after false-positive review, API authorization checks, expiration tests, and rollback have been demonstrated.

## Trust boundaries

- Treat attacker-generated logs as untrusted input. Validate and bound fields before parsing or scoring.
- Keep management interfaces separate from the decoy subnet.
- Restrict SOAR credentials and firewall API access to the minimum required actions.
- Keep audit records for classification decisions, rule creation, expiry, and operator overrides.
- Do not place real passwords, student data, or reusable production keys in the lab.

## Implementation evidence to add

When components are rebuilt, record the VM inventory, Proxmox bridge configuration, firewall policy, component versions, data formats, and successful end-to-end test results. Add sanitized screenshots or sample events only after the corresponding behavior has been verified.
