# Security Threat Model and Validation Plan

## Assets and adversaries

Protect tenant documents, prompts, model output, workload identities, exceptional secrets, reports and audit evidence. Consider malicious users, prompt-injected content, a compromised agent, compromised administrator access and configuration drift.

## Threats

| Threat | Control | Evidence required |
|---|---|---|
| Forged or replayed identity | Signature/issuer/audience/expiry validation; appropriate replay protection | Rejected invalid, expired and wrong-audience calls |
| Tenant spoofing | Server-derived context and membership check | Tenant A cannot request or overwrite tenant B records |
| Confused deputy | Check workload and delegated user authority | Authorized workload denied unauthorized user operation |
| Agent compromise | Separate hosting and identity; constrained tool access | Compromised-agent simulation cannot reach unrelated data |
| Prompt injection | Trusted policy outside model; strict tool contracts | Model-generated tenant/URL/policy overrides rejected |
| Lateral movement and exfiltration | Approved network routes, egress controls and authentication | Unapproved destination calls blocked and logged |
| Excess privilege | Minimal service data roles and periodic review | Read-only agent writes and role changes denied |
| Key exposure/outage | Managed identities, controlled Key Vault access and rotation plan | Rotation and restore exercise with redacted evidence |
| Sensitive logging | Redaction, restricted access and retention | Token, prompt and document content absent from exports |

## Planned validation

- Positive path: authorized user and workload can perform only their documented operations.
- Identity negatives: reject missing, invalid, expired and wrong-audience tokens; deny the wrong agent identity.
- Tenant negatives: reject client tenant overrides, cross-tenant point reads, writes, queries, vector retrieval and cache hits; reject missing context and mismatched queue/replay data.
- Network negatives: verify public endpoints and unapproved egress are blocked where intended; confirm private DNS paths and required platform dependencies.
- Privilege negatives: a data reader cannot write, change roles, retrieve unrelated secrets or manage the subscription.
- Telemetry: correlate allowed/denied requests and alert on suspicious activity without exporting confidential content.
- Incident exercise: isolate a compromised workload, revoke its effective access, assess leaked data and verify restoration. Account for cached tokens, role propagation and application caches.

## Current results

These tests have **not been executed** by this portfolio task. No deployed infrastructure or sanitized runtime test evidence was located. Learning completion and documentation preparation are recorded separately from implementation verification. Public exports should contain only synthetic tenant values and reviewed/redacted evidence.
