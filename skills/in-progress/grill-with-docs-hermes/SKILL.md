---
name: grill-with-docs-hermes
description: "A relentless interview that turns a loose product idea into durable repo docs before any code is written. Hermes adaptation of grill-with-docs with a full spec trail, not just CONTEXT.md and ADRs."
---

# grill-with-docs-hermes

An adaptation of [`grill-with-docs`](../engineering/grill-with-docs/SKILL.md) (which delegates to `grilling` + `domain-modeling`) for Hermes Agent workspaces. The interview style is the same: rounds of questions, wait for answers, next round. The paper trail is wider, because the user's workflow needs every decision on disk, not just vocabulary and ADRs.

## When to reach for it

- Start of a greenfield web app, game, or tool where the user has a vision but not yet a buildable spec.
- Before any implementation on a GUI-heavy or multi-session project.
- When the user says "grill me on this" and the subject is software that will live in a repo.

Do not use it for quick questions or settled scopes. Do not use it when a throwaway prototype would answer the question faster than talking.

## The paper trail

As answers resolve, write them into the project workspace immediately, not batched at the end:

| What resolved | Where it lands |
| --- | --- |
| A term with an agreed meaning | `CONTEXT.md` glossary |
| A hard-to-reverse, surprising, real trade-off decision | `docs/adr/NNNN-slug.md` |
| Product intent, users, gameplay/usage loop | `PRODUCT_SPEC.md` |
| Visual and interaction decisions | `DESIGN_SPEC.md` (plus `screenshots/reference/`) |
| What is in and out of the first build | `MVP_SCOPE.md` |
| Ordered buildable slices | `BUILD_SLICES.md` |
| Known risks and ungrillable open questions | `RISK_REGISTER.md` |
| Next executable work | `KANBAN_TASKS.md` |

Unlike upstream, resolved product and design answers do NOT stay in the conversation. The conversation is disposable; the workspace is authoritative.

## How to run it

1. Ask in rounds. Each round is the whole frontier: every question whose prerequisites are already settled. Never ask something that hinges on an unheard answer.
2. Give a recommended default first for each question so the user can accept, override, or say "I don't know."
3. The user owns scope. If they answer "agreed" to everything, push back: a session with no disagreement decided nothing.
4. Track ungrillable questions (anything about feel, look, or interaction). Do not keep rephrasing them. Mark them in `RISK_REGISTER.md` and route to a prototype/mockup instead.
5. End a round by writing resolved items to the files above. Tell the user what was written where.
6. End the session when the frontier is empty AND `MVP_SCOPE.md` + `BUILD_SLICES.md` are concrete enough to dispatch as kanban cards.

## Known weaknesses inherited from upstream, and the local fix

- **Partial loading silently drops the paper trail** (upstream bug). Local fix: this skill is self-contained and lists its own outputs. Verify files exist on disk before claiming the session produced them.
- **Most decisions stay in the conversation** (upstream design). Local fix: everything resolved lands in a named file listed above.
- **Scope too large makes questions degrade.** Local fix: if a session drifts, split the effort into smaller pieces and grill each separately (or hand off to a wayfinder-style map for multi-session efforts).

## After the session

Hand off to `autonomous-webapp-builds`: scaffold the persistent workspace, lock the dev environment (one fixed dev port), and dispatch bounded worker cards from `BUILD_SLICES.md` / `KANBAN_TASKS.md`.
