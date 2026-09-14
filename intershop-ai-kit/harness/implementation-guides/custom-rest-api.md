# Implementation Guide: Custom REST API

## Use for

Creating or additively extending an Intershop REST API.

## Request

| Required input | Answer |
|---|---|
| Capability and consumers | |
| Operation, request, response, and errors | |
| Authentication and allowed scope | |
| Validation, side effects, and limits | |
| Existing behavior to preserve | Everything unless listed |
| Compatibility/versioning constraints | |
| Acceptance evidence | |

## Required AI output

Endpoint contract; installed-version evidence; complete call flow; extension points; cartridge ownership; authorization; transaction/side-effect boundary; tests; deployment and recovery.

## Decide before implementation

- Can an existing supported endpoint satisfy the request?
- Is this an existing resource extension or a new resource? Do not create a subresource for a field.
- Which REST application, resource hierarchy, media type, version, and ACL apply?
- Which existing handler, validator, operator, mapper, repository, BO, and PO flow must be preserved?
- Which project-owned cartridges fit each layer? Use the [cartridge manual](../../docs/CUSTOM_CARTRIDGE_MANUAL.md) only for missing boundaries.
- Is physical deletion explicitly required? Otherwise use lifecycle semantics.

Apply [Architecture](../../docs/ARCHITECTURE.md), [Security](../../docs/SECURITY_CHECKLIST.md), [Quality](../../docs/QUALITY_CHECKLIST.md), and [Upgrade compatibility](../../docs/UPGRADE_COMPATIBILITY.md).

## Discover the complete flow

Locate storefront resources in installed `app_sf_rest*.jar` binaries. Also inspect `app_bo_rest_*`, `app_sf_contactcenter_rest`, `pmc_rest`, `bc_platform_rest`, and project-owned cartridges as applicable. Verify resource classes, deployed component registration, and ACLs to establish the effective path, verb, and owning cartridge.

Trace each affected verb independently:

```text
REST application/root -> ACL/auth -> resource -> trusted context
-> handler/service -> validator/operator -> repository contract
-> BO/ORM adapter -> PO/ORM -> database -> mapper -> response
```

Record:

- application/root assignment, prefix, resource hierarchy, site activation, status, headers, cache, and OpenAPI metadata;
- component contracts, implementations, instances, scopes, requirements, cardinalities, modules, and cartridge order;
- authentication providers, exact ACL path/verb rules, and trusted channel/organization/customer context;
- transactions, events, email, retries, error mapping, localization, and post-write response mapping.

## Implement

1. Define stable DTO and sanitized error contracts; never expose POs.
2. Keep resources thin and reuse the installed handler/orchestration flow. Where the installed framework supports implementation-name rebinding, register the custom implementation under the existing name in a later custom cartridge; preserve the contract and delegate existing behavior.
3. Extend at the narrowest supported seam. Prefer decorated validators, operators, and mappers that delegate existing behavior.
   When a resource obtains its handler indirectly and no narrower mapper hook exists, a project-owned resource subclass MAY override only handler resolution and delegate all request processing to the installed resource. Pair it with a handler subclass/decorator that calls `super` or its delegate before adding behavior; do not copy the resource method or replace its parent collection solely to inject the handler.
4. Validate transport and business rules before any mutation.
5. Access state through business objects and repository contracts. Keep repository implementations and all database work in PO/ORM implementation code.
6. Scope repository and PO queries using trusted context, not payload/path identity alone.
7. Define one atomic persistence operation; keep outbound side effects outside its transaction.
8. Remap authoritative stored state after mutation; do not echo the request.
9. Register resources, components, modules, mapper extensions, ACLs, application membership, cartridges, and migrations through verified project mechanisms.
10. Document contract, compatibility, limits, rollout, and recovery.

For recurring fields or variants, use one typed, business-neutral extension mechanism. Adding a variant should normally require definition/configuration and focused tests, not another endpoint or duplicated layer stack.

## Avoid

- Editing, copying, or shadowing OOTB resources.
- Bypassing an existing handler or duplicating field-specific classes.
- Database access, ORM types, or transaction control in REST/business layers.
- Assuming different verbs share authorization, validation, mapping, or side effects.
- Registering code without component instances, modules, ACLs, application membership, and assembly order.
- Returning unbounded data, internal errors, or request state after a write.

## Verify

- Contract: success, malformed input, not found, conflict, compatibility, serialization, and authoritative post-write response.
- Security: unauthenticated, forbidden, cross-scope, ACL matching, and sanitized errors.
- Wiring: verify the effective custom implementation, component delegation/cardinality, mapper execution, and endpoint behavior in every target application type.
- Unit context: use installed request-context test support (e.g., `RequestRule`) when handlers depend on the current request/channel.
- Performance: check list responses for N+1 business-object or attribute lookups.
- Persistence: rollback, concurrency/idempotency, query scope, migration, and no partial state.
- Side effects: commit ordering, failure, retry, and recovery.
- Regression: every affected existing verb and response field.

## Done

- [ ] Contract, permissions, limits, and compatibility are documented and tested.
- [ ] Installed flow is preserved and extended at supported seams.
- [ ] Layer/cartridge boundaries and transaction ownership follow Architecture.
- [ ] Registration, mapping, errors, side effects, and rollback are verified.
- [ ] No OOTB artifact changed; cross-cutting checklists pass.
