# Intershop Implementation Guides

Compact standards for developers directing AI-assisted Intershop work.

## Use

1. Select a topic in [INDEX.md](INDEX.md).
2. Complete its request table; use `Unknown` instead of guessing.
3. Give the request and guide to the AI.
4. For non-trivial work, review an [execution plan](harness/execution-plans/_TEMPLATE.md) before implementation.
5. Accept work only when the guide and linked checklists pass with evidence.

If no suitable project-owned cartridge exists, use the [cartridge manual](docs/CUSTOM_CARTRIDGE_MANUAL.md).

## Non-negotiable rules

- Do not modify, copy, replace, or shadow OOTB artifacts.
- Preserve existing behavior unless an approved requirement changes it.
- Discover installed versions and project conventions before choosing APIs or extension points.
- Keep REST separate from business/repository APIs and PO/ORM implementation.
- Access persistence through repository contracts and business objects; database work stays in PO/ORM implementation code.
- Deliver custom persistent changes through project-owned, release-versioned DBMigrate; do not change DBPrepare/DBInit.
- Record material assumptions and decisions; never invent business behavior.

See [Architecture](docs/ARCHITECTURE.md), [Security](docs/SECURITY_CHECKLIST.md), [Quality](docs/QUALITY_CHECKLIST.md), and [Upgrade compatibility](docs/UPGRADE_COMPATIBILITY.md).

## Maintain

Keep guides generic. Put ticket-specific names, payloads, paths, and acceptance data in execution plans or tests. Add a rule only for a repeatable concern, and state it once in the narrowest shared document.
