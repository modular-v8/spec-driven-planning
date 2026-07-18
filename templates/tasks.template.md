<!--
TEMPLATE FOR THE AGENT — not the deliverable.

Complex tier only. Not used at Simple (spec.md alone is the deliverable) or
Medium (plan.md is the deliverable — see tiers.md); tasks.md's phased,
dependency-tracked structure only earns its keep at Complex scale.

Phase/milestone structure; every task should carry explicit dependencies
and cite both spec and plan sections.

Every task must be:
  - Atomic: one clear unit of work, not "build the backend."
  - Independently verifiable: the Acceptance line must be checkable without
    reading the task author's mind.
  - Traceable: Refs point back to the spec/plan section that justifies it.
    A task with no traceable justification is scope creep — cut it or ask.
-->

# Tasks: {{Project Name}}

## Phase 1: {{Phase Name}}

- [ ] T1. {{Task description}}
  - Acceptance: {{how to verify this is done — a command, an output, a behavior}}
  - Refs: spec §{{n}}, plan §{{n}}
  - Depends on: {{none | Tn}}

- [ ] T2. {{Task description}}
  - Acceptance:
  - Refs:
  - Depends on:

<!-- repeat Phase N as needed -->

## Definition of Done
- [ ] All tasks above checked off
- [ ] {{project-specific final verification — e.g. "app runs locally with
      `npm run dev`", "all acceptance criteria in spec.md are met",
      "tests pass"}}
