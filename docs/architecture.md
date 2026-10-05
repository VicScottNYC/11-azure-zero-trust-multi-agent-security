# Multi-Agent Zero Trust Architecture

## Design scope

A proposed Azure design connecting specialized research, planning and reporting workloads to Foundry and tenant-aware Cosmos DB data. The design consolidates Module 11 and four security extensions. It is not a deployment record.

## Trust boundaries and request flow

1. A user authenticates through Entra ID. The ingress API validates the token and resolves membership in an application tenant; an Entra directory tenant and an application customer tenant are not necessarily the same entity.
2. The API creates immutable, server-validated tenant context and a correlation ID. Client input cannot override it.
3. Each agent runs within a suitable independent hosting boundary and obtains a destination-specific token through its own managed identity.
4. The receiving agent or service validates caller identity and permissions. Private reachability does not grant access. Check the authenticated user and application tenant where the action requires delegated user authority.
5. A tenant-aware data layer authorizes the requested operation and enforces partition/query constraints before accessing Cosmos DB. Reject missing context and mismatched document tenant values.
6. Each boundary emits sanitized authorization and operational events. Alerts feed a defined containment process.

## Authentication patterns

Managed identity is the preferred application-to-resource mechanism. It represents the workload, not the user's delegated rights. For user-authorized operations, explicitly design user-delegated OAuth or an on-behalf-of flow with the service's supported token exchange and consent. Do not forward an arbitrary upstream bearer token to a different audience. Where a legacy dependency requires keys, retrieve them using narrowly scoped Key Vault access and document rotation, expiry and fallback removal.

Workload identities are not interchangeable with interactive users: do not imply device-based Conditional Access or human JIT elevation applies uniformly to managed identities. Use service-supported workload controls and narrowly scoped permissions; reserve JIT elevation for privileged administrative access.

## Network design

Select a Container Apps environment type supporting required VNet, private endpoint, DNS and routing features. Default agent ingress to internal access. Expose only the ingress tier when necessary, with authentication and request limits. Restrict resource public access where supported and validate private name resolution from workloads.

Model allowed paths explicitly: ingress to approved agents; research to knowledge resources; planning to planning data; reporting to report data; required identity, registry, DNS and telemetry endpoints. Apply supported firewall/routing controls for egress. Review service-required network dependencies before blocking traffic.

Container apps in a shared environment share a virtual network. Subnet controls should not be presented as guaranteed per-agent segmentation. Use separate environments/trust zones when stronger separation is required. Dapr components, if introduced, must be explicitly scoped; mTLS transport does not eliminate authorization requirements.

## Data and encryption

Use a trusted tenant key across records, retrieval indexes, caches and messages. Validate tenant membership independently from resource RBAC. Use container/account separation for stronger data boundaries, with appropriate workload routing and role scopes. Enable TLS, retain service encryption at rest, and consider CMK with Key Vault only after assessing rotation and key availability requirements.

## Operational ownership

Platform owners manage identities, private endpoints and diagnostic settings; application owners enforce tenant policy and tool contracts; security owners review access, alerts and incidents. Actual deployment must supply owners, retention periods, recovery objectives and evidence without placing private operational identifiers in the public repository.
