# ADR-0004: Mandatory documentation update on every change

- Status: Accepted
- Date: 2026-04-27

## Context

Brief from the CEU board: "this repo will be developed by agents receiving prompts from non-technical users, so documenting every single change and spec and feature is MANDATORY."

We need to encode this directive so that every future agent reads it and complies.

## Decision

`AGENTS.md` declares, at the top of the file:

> **DOCUMENTING EVERY SINGLE CHANGE, SPEC, AND FEATURE IS MANDATORY.**
> No exceptions. If your change does not include a documentation update, the change is not done.

The mandate is repeated in:

- `CONTRIBUTING.md` — for humans
- `docs/checklists/pre-commit.md` — first item on the list
- The PR template (when added) — must be checked

A change is considered "documented" when **at least one** of the following has been updated:

- A spec in `docs/specs/`
- A new ADR in `docs/decisions/`
- `docs/content.md`
- `docs/architecture.md`
- `BRAND.md`
- `CHANGELOG.md` (always)

## Consequences

### Positive
- New agents arriving with no context find a clear, repeated rule.
- Non-technical maintainers can review PRs at the doc level even if they can't read code.
- The repo stays self-explanatory across personnel turnover.

### Negative
- A small fraction of trivial changes (e.g. typo fixes) still incur a CHANGELOG row. We accept this — it's the simplest rule that survives contact with non-technical reviewers.

## Enforcement
- Pre-commit checklist asks for it.
- Future: a GitHub Action could fail PRs that don't touch any `docs/**` or `CHANGELOG.md`. Out of scope for v1.
