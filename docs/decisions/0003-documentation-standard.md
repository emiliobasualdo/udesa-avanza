# ADR-0003: Documentation standard for an agent-maintained repo

- Status: Accepted
- Date: 2026-04-27

## Context

This repo will be maintained by AI agents receiving prompts from non-technical CEU members. We need a documentation system that:

1. **Has a single, obvious entry point** for any new agent.
2. **Forces every change to be documented**, otherwise context decays.
3. **Separates the four kinds of doc** users need: how to start, how to do X, what each thing is, why we did it.
4. **Captures decisions** so future agents don't re-litigate them.
5. **Stays low-effort** — no fancy generators.

## Decision

Combine four well-known industry standards:

| Concern | Standard | Where |
|---|---|---|
| Agent entry point | [agents.md](https://agents.md) | `AGENTS.md` |
| Doc structure | [Diátaxis](https://diataxis.fr) | `docs/` |
| Decisions log | [ADR](https://adr.github.io) | `docs/decisions/` |
| Changelog | [Keep a Changelog](https://keepachangelog.com) | `CHANGELOG.md` |
| Commits | [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) | every commit |

**Mandatory rule** (enforced by `AGENTS.md`): no PR is mergeable without an accompanying doc update.

## Consequences

### Positive
- Each agent landing in the repo finds `AGENTS.md` first, learns the rule, and applies it.
- Diátaxis avoids the common trap of mixing tutorial and reference.
- ADRs prevent decision drift.
- Conventional Commits give us a free, parseable history.

### Negative
- Slightly higher friction per change (≈30 s extra to update a doc).
- We pay this cost on purpose — the alternative is unmaintainable context.

## Alternatives rejected
- **README-only**: scales badly past ~3 features.
- **Wiki on GitHub**: not version-controlled with the code; agents can't edit it as easily.
- **Storybook / Notion**: overkill for a single static page.
