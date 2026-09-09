# Implementation Guide: <Category>

## Use for

<One repeatable business outcome.>

## Request

`Unknown` means discover or request a decision; never guess.

| Required input | Answer |
|---|---|
| Outcome and consumers | |
| Normal and exceptional behavior | |
| Inputs, outputs, and side effects | |
| Access, privacy, and limits | |
| Existing behavior to preserve | Everything unless listed |
| Explicit non-goals or removals | |
| Acceptance evidence | |

## Required AI output

- Installed-version and repository evidence
- Supported extension point and affected boundaries
- Decisions, assumptions, exact change plan, tests, rollout, and recovery

## Decide before implementation

- <Material decision gate>

Apply [Architecture](../../docs/ARCHITECTURE.md), [Security](../../docs/SECURITY_CHECKLIST.md), [Quality](../../docs/QUALITY_CHECKLIST.md), and [Upgrade compatibility](../../docs/UPGRADE_COMPATIBILITY.md).

## Workflow

1. <Discover>
2. <Design>
3. <Implement>
4. <Register/migrate>
5. <Verify/document>

## Avoid

- <Recurring failure>

## Verify

- <Observable test>

## Done

- [ ] Category-specific contract and behavior are verified.
- [ ] No OOTB artifact is changed or shadowed.
- [ ] Existing behavior is preserved unless explicitly approved otherwise.
- [ ] Cross-cutting checklists pass.
