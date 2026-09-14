# Custom Cartridge Manual

Use only when no existing project-owned cartridge has the required responsibility.

## Inputs

| Decision | Required evidence |
|---|---|
| Responsibility and name | Project naming and layer convention |
| Package | Organization-owned namespace; never `com.intershop.*` |
| Target assemblies/environments | Where the capability must run |
| Dependencies and extension point | Minimal installed-version precedent |
| Persistent changes | Introducing release and DBMigrate convention |

## Rules

- Never create in, copy, or shadow an OOTB cartridge.
- Create a cartridge for a coherent layer/lifecycle, not per endpoint or field.
- Keep REST separate from business/repository APIs and PO/ORM implementation.
- Put repository contracts in business/API code. Keep implementations with PO/ORM unless project precedent requires another cartridge.
- Preserve one-way dependencies and existing cartridge order.
- Place a custom extension cartridge after the cartridges it extends or decorates, normally last among the relevant production cartridges. Where the framework supports order-based overrides, the later cartridge takes precedence over earlier cartridges; order alone does not override all functionality or replace explicit dependencies and extension wiring.

## Procedure

1. Search project-owned cartridges; record why none fits.
2. Inspect project inclusion, two comparable build files, cartridge metadata, dependency injection, target assemblies, order, and migration conventions.
3. Create only the directories and configuration required by that precedent.
4. Use the installed build plugin and minimal supported API dependencies; do not copy a large dependency list.
5. Register the subproject when discovery is not automatic.
6. Include the custom cartridge in each target assembly's `build.gradle` using its packaging and ordering mechanism (e.g., `storefrontCartridges`, `include`, and `order`), and declare required build dependencies. Packaging and application activation are separate requirements.
7. Self-register from the custom cartridge's `staticfiles/cartridge/components/apps-extension.component` (or the filename used by project precedent). Fulfill `selectedCartridge` for each application type required by its function; do not edit default application-suite cartridges. Register other applicable components, modules, REST membership, ACLs, mappers, services, ORM, and activation.
8. Put persistent changes in a project-owned DBMigrate step versioned for the introducing release; never use DBPrepare/DBInit for delivery.
9. Build, test, assemble, deploy, and prove framework discovery.

Exact plugins, paths, tasks, annotations, and resource formats must come from the target project and installed version.

## Verify

- Build recognizes the project and produces its artifact.
- Verify packaging and order in the generated `cartridgelist.properties`, and application membership for every target application type.
- The resolved cartridge order places it after every extended or decorated cartridge, and the intended override takes effect while unrelated behavior is preserved.
- Server startup and component/resource discovery succeed without collisions.
- Authorization and existing behavior regressions pass.
- Migrations upgrade every supported prior release and converge on fresh setup.
- Disabling the custom capability restores previous behavior without an OOTB edit.

## Done

- [ ] The cartridge has one justified responsibility and project-owned identity.
- [ ] Build, registration, order, dependencies, and discovery follow verified precedent.
- [ ] Layer boundaries match [Architecture](ARCHITECTURE.md).
- [ ] Persistent changes use release-versioned DBMigrate only.
- [ ] Tests and deployment evidence pass; no OOTB artifact changed.
