# Security Checklist

- [ ] Every external input is validated server-side for type, size, range, format, and allowed values.
- [ ] Authentication mechanism and API/service audience are explicit.
- [ ] Authorization is checked for every operation, including reads.
- [ ] Repository lookups are scoped to the trusted organization, channel, application, site, and customer context.
- [ ] Client-provided identifiers are not treated as authorization.
- [ ] Queries are parameterized; user input is never concatenated into a query.
- [ ] Errors expose no stack trace, SQL, internal IDs, tokens, secrets, or personal data.
- [ ] Logs redact credentials, cookies, tokens, payment information, and sensitive personal data.
- [ ] Secrets use the approved secret store and are never committed or placed in frontend bundles.
- [ ] Outbound calls use TLS verification, explicit timeouts, bounded retries, and destination allow-listing where applicable.
- [ ] Webhook/event authenticity and replay protection are implemented where applicable.
- [ ] CORS, rate limiting, payload limits, and abuse controls are defined for public APIs.
- [ ] Data classification, retention, deletion, masking, and access requirements are recorded for BI/migration workloads.
- [ ] Generated code and dependencies pass the project's security and license checks.
