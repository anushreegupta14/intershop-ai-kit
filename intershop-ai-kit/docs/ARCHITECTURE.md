# Architecture

## Invariants

- Never modify, copy, replace, rename, or shadow an OOTB artifact.
- Extend through verified hooks, bindings, composition, or delegation in project-owned code.
- Preserve existing behavior by default. Breaking changes require explicit approval, impact, migration, versioning, and recovery plans.
- Keep dependencies one-way and reject circular cartridge dependencies.

## Backend flow

```text
transport -> handler/service -> validator/operator -> repository contract
          -> business object/ORM adapter -> PO/ORM -> database
```

| Layer | Responsibility |
|---|---|
| Transport | Parse shape, obtain trusted context, invoke use case, map response |
| Handler/service | Authorize, apply business rules, orchestrate |
| Validator/operator | Validate before mutation; apply validated intent |
| Repository contract | Domain-facing lookup and atomic operation boundary |
| Business object/ORM adapter | Business behavior without leaking persistence objects |
| PO/ORM | Repository implementation, queries, mappings, POs, database access |
| DTO/mapper | Separate transport contracts from domain and persistence types |

Transactions follow the atomic repository operation and are implemented using project PO/ORM conventions. Never hold one across an outbound call.

## Cartridge boundaries

- REST must not own repository implementations, POs, ORM mappings, queries, or migrations.
- Put repository and business-object contracts in a business/API cartridge.
- Put repository implementations with PO/ORM unless verified project conventions require a separate implementation cartridge.
- Business and persistence cartridges must not depend on REST types.
- Reuse a project-owned cartridge only when its responsibility fits; otherwise create and register one.

## Database lifecycle

- Runtime database access occurs only in PO/ORM implementation code behind repositories.
- Deliver every custom schema, preference, reference-data, or persistent-configuration change through a project-owned migration versioned for the introducing release.
- Never edit an OOTB migration or use DBPrepare/DBInit for feature delivery.
- Verify upgrade ordering, supported starting versions, failure recovery, and fresh-install convergence through the migration chain.

## Frontend

- Keep custom UI in project-owned components, libraries, themes, or storefront layers.
- Compose OOTB behavior; do not patch installed packages.
- Separate CMS, API, mapping, presentation, and analytics concerns.
- Preserve accessibility, localization, SEO, telemetry, and behavior.

## External services

- Own the contract and adapter; define system of record and data classification.
- Use least privilege, timeouts, bounded retry, idempotency, and circuit breaking where applicable.
- Define observability, ownership, deployment, and recovery.
