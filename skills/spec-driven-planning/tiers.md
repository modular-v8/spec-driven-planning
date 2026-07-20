# Complexity Tiers

This file defines the three complexity tiers the skill asks the user to choose
between at the start of the flow, and what each tier controls downstream:
which artifacts get generated, how deep the question sets go, and how many
human checkpoints happen before the coding agent is handed off to.

The tier is a starting point, not a cage. If answers during elicitation reveal
the project is bigger or smaller than expected, re-propose a tier change to
the user before continuing (e.g. "this touches auth and a database — want to
bump to Medium/Complex?"). Always let the user override the tier directly too.

---

## Simple

**Use when:** a single-purpose script or tool, one component, no real
architecture decision to make, throwaway or personal use, output is something
a coding agent could plausibly finish in one sitting.

**Examples:** a CLI script to rename/organize files, a small scraper, a one-off
data-cleanup script, a browser bookmarklet, a single Python function with a
test.

**Artifacts generated:**
- `spec.md` — and only spec.md. Sections: outcome, in scope, out of scope,
  constraints, prior decisions, requirements (flat list — see below), no
  acceptance criteria section.
- `plan.md` and `tasks.md` are both **skipped**. A project small enough for
  a 5-question elicitation doesn't need a separate architecture doc or task
  breakdown — spec.md's `requirements` section already tells a coding agent
  what "done" looks like.

**Requirements format:** unlike Medium/Complex, Simple's `requirements`
section is a flat, unlabeled list mixing always-active ("The system
SHALL...") and event-driven ("WHEN..., the system SHALL...") statements
together — no subsection headers for those two. `unwanted behavior` stays
its own subsection. No separate acceptance-criteria/pass-fail list — the
requirements themselves are the completion bar at this tier.

**Question depth:** up to 5 questions, hard cap (not a target) — single
stage, spec only.

**Checkpoints:** 1 — show the user spec.md, get a go-ahead, then hand off
directly. No plan.md, no tasks.md, no second review gate.

---

## Medium

**Use when:** a multi-file feature or small application, some real design
decisions exist (data model, framework choice, a couple of integrations),
built for a single user or small team, realistically days (not hours) of
coding-agent work.

**Examples:** a personal finance tracker web app, a REST API with a database,
a CLI tool with subcommands and config, a browser extension with multiple
components.

**Artifacts generated:**
- `spec.md` (full — adds users & context, data & integrations)
- `plan.md` (moderate depth — Approach, Architecture, Tech Stack, Data Model,
  File/Module Structure)
- `tasks.md` is **not** generated at Medium — it's reserved for Complex,
  where a phased breakdown with dependencies actually earns its keep.
  plan.md's File/Module Structure plus spec.md's requirements are enough
  for a coding agent to work from directly at this tier.

**Question depth:** up to 10 questions, hard cap (not a target) — two
stages (spec, then plan), no separate tasks stage. Split: ≤5 spec, ≤5 plan.

**Checkpoints:** 2 — review spec.md, then review plan.md, then hand off.
No tasks.md generation step.

---

## Complex

**Use when:** a multi-component system, real integrations/auth/persistence/
scaling concerns, multiple stakeholders or a long maintenance horizon,
realistically weeks of coding-agent work, anything production-bound.

**Examples:** a multi-tenant SaaS platform, a production API with auth and
billing, a system integrating several external services, anything with
compliance or uptime requirements.

**Artifacts generated:**
- `spec.md` (full — adds risks & open questions; non-functional requirements
  fold into the `requirements` section as SHALL/WHEN/IF clauses rather than
  a separate section)
- `plan.md` (deep — adds Integration Plan, Security & Error Handling
  Strategy, Sequencing & Milestones, Alternatives Considered)
- `tasks.md` (phased/milestone-based, with explicit dependencies and
  acceptance criteria per task)

**Question depth:** up to 20 questions, hard cap (not a target) — three
stages, with any headroom under the cap reserved for follow-up probing
(don't just fire the list — ask a follow-up when an answer is vague, e.g.
"you said 'needs to scale' — roughly what load?") rather than new topics.
This is a self-contained set, not Medium's questions plus additions — at
Medium's own cap of 10, there's no room left to also inherit them wholesale,
so complex.md folds the same topic coverage into its own question list.
Split: ≤8 spec, ≤7 plan, ≤3 tasks (2 questions of headroom for follow-ups).

**Checkpoints:** 3+ — review spec.md, review plan.md, then review tasks.md
phase-by-phase (confirm Phase 1 before generating Phase 2's detail) rather
than dumping the whole task list at once. This is the tier where handing a
coding agent an un-reviewed 40-item task list is most likely to go sideways.

---

## Quick reference

| | Simple | Medium | Complex |
|---|---|---|---|
| spec.md | light, no acceptance criteria | full | full |
| plan.md | — | moderate | deep |
| tasks.md | — | — | phased |
| Questions | ≤5 (hard cap) | ≤10 (hard cap) | ≤20 (hard cap) |
| Checkpoints | 1 | 2 | 3+ |
