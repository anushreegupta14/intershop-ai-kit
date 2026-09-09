# Intershop Implementation Guide Index

Choose the business outcome. You do not need to know the technical solution or write a sophisticated prompt.

| # | Area | Business outcome | Guide | Complexity | Phase |
|---:|---|---|---|---|---|
| 1 | Backend | Create or extend an Intershop REST API for headless commerce | [Custom REST API](harness/implementation-guides/custom-rest-api.md) | Low–Medium | 1 |
| 2 | Backend | Generate and transform commerce data for BI/reporting | [Commerce BI Data](harness/implementation-guides/commerce-bi-data.md) | Low–Medium | 1 |
| 3 | Backend | Generate Intershop preferences and configuration safely | [Preference Generator](harness/implementation-guides/preference-generator.md) | Low–Medium | 1 |
| 4 | Frontend | Create an Intershop Angular PWA component backed by Contentful CMS | [Angular PWA CMS Component](harness/implementation-guides/angular-pwa-cms-component.md) | Low–Medium | 1 |
| 5 | Backend | Design and implement a serverless commerce microservice | [Serverless Microservice](harness/implementation-guides/serverless-microservice.md) | Medium–High | 1 |
| 6 | Frontend | Add upgrade-compatible capabilities to an Intershop PWA baseline | [PWA Accelerator](harness/implementation-guides/pwa-accelerator.md) | High | 1 |
| 7 | Frontend | Migrate legacy ISML behavior to a Next.js storefront | [Legacy ISML to Next.js](harness/implementation-guides/legacy-isml-to-nextjs.md) | High | 2 candidate |

## Always apply

| Standard | Purpose |
|---|---|
| [Architecture](docs/ARCHITECTURE.md) | OOTB protection and mandatory layer boundaries |
| [Upgrade compatibility](docs/UPGRADE_COMPATIBILITY.md) | Safe extension and upgrade rules |
| [Security checklist](docs/SECURITY_CHECKLIST.md) | Authentication, authorization, secrets, privacy, and integration safety |
| [Quality checklist](docs/QUALITY_CHECKLIST.md) | Tests, compatibility, operability, and acceptance evidence |
| [Custom cartridge manual](docs/CUSTOM_CARTRIDGE_MANUAL.md) | Create and register a project-owned cartridge when no suitable one exists |

## If no guide matches

Copy [the guide template](harness/implementation-guides/_TEMPLATE.md), create one focused guide, and add it to this index. Do not stretch an unrelated guide to fit.
