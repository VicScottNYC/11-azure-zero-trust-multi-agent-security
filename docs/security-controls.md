# Identity, Network and Data Controls

All controls below are proposed implementation requirements.

## Extension 1 — Zero Trust security model

Every access boundary authenticates and authorizes the caller. Minimize standing privileges and prepare for a compromised agent. Agent output and retrieved content are untrusted data, including instructions attempting to change policy or select a different tenant.

## Extension 2 — Managed identities and RBAC

Use separate managed identities for separately hosted agents. System-assigned identity follows the hosting resource lifecycle; a user-assigned identity has an independent lifecycle and can be attached to multiple resources, so review assignments to prevent unintended sharing.

Inventory destination, required action, identity, role and scope. Distinguish Azure resource management permissions from service-specific data access. Research should receive read-only knowledge access; planning only the planning-record operations; reporting only the report operations. Where built-in data roles are broader than necessary, assess supported custom roles. Do not grant Owner or Contributor merely to resolve authentication failures.

An identity shared by multiple agent instances is appropriate only within the intended same trust boundary. Separate identities attached to one shared runtime may all be obtainable by that runtime and do not prove agent isolation. Review and remove stale role assignments and audit changes.

## Extension 3 — Container Apps networking/security

Plan ingress authentication, internal visibility, private endpoints and DNS, environment isolation, supported subnet controls, required egress and firewall routes. Check plan-specific networking support. Validate destination TLS and certificate trust. Client certificate forwarding requires application validation; mTLS is transport identity and still needs authorization. Avoid claiming that a shared Container Apps environment enforces per-agent NetworkPolicy. Kubernetes is outside this portfolio scope.

## Extension 4 — Cosmos DB security and tenant isolation

Use Cosmos DB for NoSQL native data-plane RBAC scoped to approved containers/databases. A partition key is not a native RBAC authorization boundary. A shared-container application must enforce tenant membership and allowed operations itself. For stronger security requirements, use separate containers/accounts and limit each workload's role scope accordingly.

Derive tenant context from verified identity/membership. Check context on point reads, tenant-partition queries and writes; prevent cross-partition queries in the exposed application interface. Parameterize queries and validate item ownership. Include tenant identity in cache keys, vector filters, queue envelopes, generated reports and replay checks. Treat returned agent/tool tenant values as untrusted.

Cosmos DB encrypts data at rest; require TLS in transit. CMK introduces key lifecycle and availability responsibilities and is an optional design choice, not a claimed implemented feature. Key Vault access must be narrowly authorized and audited. Rotation and restoration require tested procedures.

## Observability, risks and compliance

Configure relevant service diagnostics; application authorization logs should include operation, decision, correlation and pseudonymous tenant context without credentials or sensitive payloads. Monitor unusual denies, role changes and outbound destinations. Set retention and restricted log access, then exercise incident containment.

Map regulatory obligations to real controls and evidence with qualified organizational review. The learner's module completion does not establish SOC 2 certification or compliance with privacy or AI regulation.
