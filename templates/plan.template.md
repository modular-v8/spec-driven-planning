<!--
TEMPLATE FOR THE AGENT — not the deliverable.

Skipped entirely at Simple tier (its content folds into tasks.md instead).

Tier tags:
  tier: medium+   -> include at Medium and Complex
  tier: complex   -> include at Complex only

Every decision in this file should trace back to something in spec.md
(a requirement, a constraint, a non-functional requirement). If a plan
decision doesn't map to anything in the spec, that's a sign either the spec
is missing something or the plan is over-building — flag it to the user
rather than silently resolving it.
-->

# Plan: {{Project Name}}

<!-- tier: medium+ -->
## Approach Summary
One paragraph: the overall technical strategy in plain language, before any
detail. Someone should be able to read just this paragraph and know roughly
what's being built and how.

<!-- tier: medium+ -->
## Architecture
Components/modules and how they interact. A bullet tree or ASCII sketch is
fine at Medium; a proper diagram (or a description precise enough to draw
one) is expected at Complex.

<!-- tier: medium+ -->
## Tech Stack & Key Decisions
Chosen languages/frameworks/libraries with a one-line rationale each. Only
decide what spec.md's Constraints left open — don't relitigate anything
already fixed there.

| Decision | Choice | Why |
|---|---|---|
| | | |

<!-- tier: medium+ -->
## Data Model
Entities, relationships, and storage choice. Enough detail that a coding
agent could generate a schema from it without guessing.

<!-- tier: medium+ -->
## File / Module Structure
Proposed directory layout or component breakdown. This is what the coding
agent will treat as its scaffold — be as concrete as the tier warrants.

<!-- tier: complex -->
## Integration Plan
Order of integration for external services, and a mock/stub strategy for
local development so the coding agent isn't blocked on live credentials
during the build.

<!-- tier: complex -->
## Security & Error Handling Strategy
AuthN/AuthZ approach, input validation strategy, failure modes and how they
surface (exceptions, logs, user-facing messages), secrets handling.

<!-- tier: complex -->
## Sequencing & Milestones
Phases of build-out and the dependencies between them. This becomes the
phase structure in tasks.md — keep them aligned.

<!-- tier: complex -->
## Alternatives Considered
Options that were rejected and why. This exists so the coding agent (or a
future human) doesn't re-open a decision that was already made deliberately.

<!-- tier: complex -->
## Open Risks Carried From Spec
Link back to spec.md's Risks & Open Questions — note which ones this plan
resolves, mitigates, or still leaves open.
