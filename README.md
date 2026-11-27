# Project ADEPT

**Autonomous Deception and Endpoint Threat Intelligence** is a planned defensive security lab. It explores how an isolated honeypot can produce telemetry for analysis and how a guarded response service could contain high-confidence activity.

The current design uses Proxmox VE to host an attacker VM and segmented defensive services: Cowrie as an SSH decoy, Splunk as the SIEM, a Python classifier, and a FastAPI-based SOAR controller. The intended event path is documented in [Architecture](docs/architecture.md).

> **Current status:** The Proxmox project drive was reported corrupted, and the guest VMs are unavailable. This repository currently contains documentation only.

## Documentation

- [Architecture and data flow](docs/architecture.md)
- [Isolated lab network plan](docs/network-plan.md)
- [Rebuild and verification checklist](docs/setup.md)
- [Threat model and safeguards](docs/threat-model.md)
- [Project status and roadmap](docs/roadmap.md)

## Design goals

- Keep all testing inside a dedicated, isolated virtual lab.
- Send honeypot telemetry to a central SIEM for analysis.
- Treat classification and automated blocking as proposed capabilities until they are implemented and tested.
- Make containment reversible with short-lived rules and auditable changes.

## Safety boundary

Do not connect the decoy network to the campus network or expose it to the public Internet. Use disposable lab machines and synthetic credentials. Do not run scans or attack simulations against systems outside the isolated lab unless you have separate written authorization.

## Repository status

No VM definitions, application source, Splunk configuration, or deployment scripts are present yet. Documentation describes the intended design and the evidence required before claiming a feature works.
