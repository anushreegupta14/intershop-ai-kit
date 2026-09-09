# Agent Entry-Point

Add this instruction to the repository's coding-agent rules:

```markdown
For Intershop work:

1. Select and read the matching `intershop-implementation-guides/INDEX.md` guide and all standards it links.
2. Treat `Unknown` as a discovery or decision item; never invent business behavior.
3. Inspect installed versions, existing behavior, project precedent, and supported extension points before designing.
4. Use project-owned code only; never modify, copy, replace, or shadow OOTB artifacts.
5. Preserve behavior unless an explicit approved requirement changes it.
6. Keep REST, business/repository APIs, and PO/ORM implementation within the boundaries defined by `docs/ARCHITECTURE.md`.
7. Deliver persistent changes through project-owned, release-versioned DBMigrate; do not change DBPrepare/DBInit.
8. For non-trivial work, create and obtain required review of `harness/execution-plans/_TEMPLATE.md` before implementation.
9. Implement, verify, and report evidence against the guide and security, quality, and upgrade checklists.
```
