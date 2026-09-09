# Operating Model

An implementation guide defines reusable rules for one work category. An execution plan applies those rules to one ticket.

| Artifact | Contains | Excludes |
|---|---|---|
| Guide | Stable inputs, decisions, workflow, risks, checks | Ticket-specific design |
| Execution plan | Evidence, exact changes, assumptions, tests, rollout | New reusable policy |

## Workflow

1. Select a guide from the index.
2. Complete its request table; `Unknown` is valid.
3. Inspect installed code, versions, and project precedent.
4. Record exact design and unresolved decisions in an execution plan.
5. Obtain required review, implement, and verify.
6. Report changed behavior, evidence, risks, and follow-up.

Never infer missing business behavior. Discover it or request a decision.

## Maintenance

- One guide per repeatable category.
- Keep rules precise, generic, and testable.
- Link shared standards instead of repeating them.
- Update a guide only for a recurring review lesson.
- Keep secrets and customer-specific data out of guides.
