# Threat model and safeguards

## Scope

This model covers the proposed isolated Proxmox lab: Kali, Cowrie, Splunk, the classifier, the SOAR service, and the virtual firewall. The real campus network, production accounts, and public-facing services are out of scope.

The threats below are design risks from the review materials. Controls are proposed until implementation evidence is added.

| STRIDE category | ADEPT risk | Proposed safeguards |
| --- | --- | --- |
| Spoofing | A scanner may identify Cowrie from protocol behavior or reused host keys. | Use unique keys per instance, review protocol behavior, and avoid claiming the decoy is indistinguishable without testing. |
| Tampering | Crafted events or poisoned input may distort the classifier baseline. | Validate and bound inputs, restrict baseline write access, version datasets, and review changes. |
| Repudiation | An automated block may be difficult to explain or audit. | Record source, triggering evidence, model/version, decision, rule ID, expiry, and any operator override. |
| Information disclosure | Static or reused keys and exposed management services can reveal lab details. | Use synthetic credentials, unique keys, and management access restricted to a separate admin path. |
| Denial of service | High event volume can exhaust firewall state or SIEM ingest capacity. | Rate-limit test traffic, cap event sizes and ingest, use short-lived blocks, and monitor resource limits. |
| Elevation of privilege | A flat virtual bridge or overpowered SOAR credential can enable lateral movement. | Segment subnets, default-deny inter-zone traffic, restrict API actions, and use least-privilege credentials. |

## Automated response safeguards

- Start with observe-only decisions and review false positives.
- Restrict automatic actions to known lab source ranges and an explicit allowlist.
- Set a short expiration time for temporary rules and verify expiration works.
- Log each decision and rule change; protect the audit log from the SOAR service account where practical.
- Rate-limit rule creation and cap the number of active blocks.
- Keep a tested operator override and rollback path.
- Do not allow the classifier to modify its own baseline or SOAR policy.

## Risk scoring

The review deck lists firewall exhaustion, model poisoning, and a flat bridge as risks, but the scoring rubric is not documented here. Define likelihood and impact scales before publishing numeric ratings. Reassess risks after controls are implemented and tested.
