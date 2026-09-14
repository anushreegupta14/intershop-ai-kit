# Upgrade Compatibility Checklist

- [ ] Installed ICM/PWA/Angular/Next.js and dependency versions were discovered, not assumed.
- [ ] No OOTB cartridge or installed dependency source was modified or shadowed.
- [ ] The supported extension point is named in the execution plan.
- [ ] Custom code is isolated in project-owned locations.
- [ ] Existing public behavior and contracts have regression coverage.
- [ ] Version-specific APIs are verified against installed code or authoritative documentation.
- [ ] Do not use or extend deprecated APIs, components, or features when a supported replacement is available in the target version. Verify deprecation status and use the replacement for new implementation work; if none exists, document the constraint and migration path.
- [ ] Custom configuration has defaults and remains valid when the new feature is disabled.
- [ ] Data/schema changes are forward-compatible and have a safe rollback or roll-forward strategy.
- [ ] New tables, columns, preferences, reference data, and all other custom persistent changes use project-owned, release-versioned DBMigrate; DBPrepare/DBInit remain setup-only and unchanged.
- [ ] Upgrade conflicts can be detected by automated tests rather than manual comparison alone.
- [ ] Any explicitly approved breaking change has consumer impact, migration, release, and rollback notes.
