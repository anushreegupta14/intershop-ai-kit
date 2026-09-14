# Implementation Guide: Intershop Preference

## Use for

Creating or extending a project-owned preference definition, typed access, initial values, localization, and optional REST exposure. Changing only an environment value is configuration management.

## Request

| Required input | Answer |
|---|---|
| Configurable behavior and owner | |
| Key, type, constraints, scope, and safe default | |
| Initial scopes and editable roles | |
| Sensitivity and environment variance | |
| REST consumer/exposure | No by default |
| Existing behavior to preserve | Everything unless listed |
| Acceptance evidence | |

## Required AI output

Installed-version precedent; collision search; owning cartridge; preference contract; DBMigrate release; typed access boundary; REST/whitelist decision; tests; rollout, recovery, and upgrade reconciliation.

## Decide before implementation

- Reuse a preference only when semantics, type, and scope match.
- Do not store secrets, personal data, or security-sensitive controls as ordinary preferences.
- REST exposure is opt-in and requires a named consumer, safe disclosure, authorization, serialized form, and cache behavior.
- Discover exact preference, migration, localization, scope, and whitelist mechanisms from the installed version; do not copy generic syntax.

Apply [Architecture](../../docs/ARCHITECTURE.md), [Security](../../docs/SECURITY_CHECKLIST.md), [Quality](../../docs/QUALITY_CHECKLIST.md), and [Upgrade compatibility](../../docs/UPGRADE_COMPATIBILITY.md).

## Implement

1. Search installed and project code for the key, equivalent semantics, suitable group, access service, migrations, and active REST whitelist.
2. Define stable key, group, installed-framework type, scope, constraints, safe default, missing behavior, editability, localizations, owner, and retirement path.
3. Add group, definition, and required initial values through a project-owned DBMigrate step versioned for the introducing release. Never edit released/OOTB migrations or DBPrepare/DBInit.
4. Centralize reads, conversion, validation, trusted-scope resolution, and fallback in one typed configuration service. Route writes through a repository using supported APIs.
5. If REST exposure is approved, identify the exact whitelist resource, consumer, qualified key, and merge/replacement behavior.
6. Prefer additive whitelist contribution. If a complete overlay is required, preserve all effective entries, record its upstream version, and add automated upgrade reconciliation.
7. Verify endpoint authentication separately; whitelisting is serialization, not authorization.
8. Document operator procedure, propagation/cache/restart behavior, migration, recovery, and ownership.

## Avoid

- Duplicate keys, guessed type/scope codes, unsafe defaults, or scattered raw-key access.
- Direct SQL or preference writes from REST/services.
- Editing the OOTB whitelist or assuming resources merge.
- Replacing a whitelist with only the custom entry.
- Exposing sensitive values or weakening endpoint authorization.

## Verify

- Migration from every supported release and fresh-install convergence.
- Type, constraints, localization, default/missing behavior, scope precedence, isolation, and writes.
- When exposed: exact serialized value, caller permissions, cache behavior, preservation of existing entries, and absence of unapproved/sensitive values.
- Upgrade comparison for any copied upstream resource.
- Disabled/default behavior and existing configuration regressions.

## Done

- [ ] Contract, ownership, precedent, and collision search are recorded.
- [ ] Versioned DBMigrate delivers all persistent artifacts.
- [ ] Typed access centralizes scope, validation, and fallback.
- [ ] REST exposure and whitelist behavior are explicitly justified and tested.
- [ ] Upgrade reconciliation and recovery are defined where needed.
- [ ] No OOTB artifact changed; cross-cutting checklists pass.
