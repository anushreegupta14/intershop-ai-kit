# Quality and Delivery Checklist

- [ ] Acceptance criteria are measurable and traceable to tests.
- [ ] Assumptions and unresolved decisions are visible in the execution plan.
- [ ] Existing functionality remains covered by regression tests.
- [ ] Unit tests cover business decisions and error paths.
- [ ] Integration/contract tests cover framework registration and boundaries.
- [ ] End-to-end or smoke tests cover the critical user/business journey.
- [ ] Failure, retry, concurrency, idempotency, and rollback behavior are tested where relevant.
- [ ] Every custom persistent change is delivered by a project-owned DBMigrate step versioned for the introducing release; no custom DBPrepare/DBInit change is used.
- [ ] DBMigrate is tested from every supported prior project release, including ordering, failure recovery, and convergence of a fresh setup through the migration chain.
- [ ] Representative data volume/performance limits are defined and verified where relevant.
- [ ] Accessibility, localization, SEO, responsive behavior, and analytics are checked for storefront changes.
- [ ] Logs, metrics, traces, correlation IDs, alerts, and operational ownership are defined.
- [ ] Documentation includes configuration, examples, deployment, rollback, and troubleshooting.
- [ ] No generated placeholder, fabricated import/API, secret, debug code, or disabled test remains.
- [ ] Dependency changes reuse platform-compatible artifacts without duplicate classes; run each affected assembly's `checkClassCollisions` task where available. Clean stale generated output and recheck if collisions persist after a dependency correction.
- [ ] Build, lint, tests, and repository-specific verification commands pass.
