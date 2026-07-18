---
name: spec-driven-planning
description: Turns a rough project idea into spec.md, plan.md, and tasks.md files BEFORE any code gets written, so a coding agent gets a clear, human-reviewed blueprint instead of a single freeform prompt. Explicit-invocation only — trigger ONLY when the user directly names this skill (e.g. types "/spec-driven-planning") or explicitly asks to "use the spec-driven-planning skill." Do NOT self-trigger from general project descriptions, planning requests, or phrases like "plan this out" / "spec this out" / "help me scope this" — those alone are not sufficient, even though they sound related. Wait to be invoked by name.
---

# Spec-Driven Planning

## Why this exists

One-shot prompting a coding agent on anything beyond a trivial task tends to
produce sprawling, inconsistent output — the agent is inferring scope,
architecture, and "done" as it goes, with no chance for the human to correct
course until the code already exists. Producing `spec.md` (what/why),
`plan.md` (how), and `tasks.md` (checklist) *before* handing off to a coding
agent forces those decisions up front, with the human confirming each one.
This skill runs that process end to end.

## Before you start

Read [tiers.md](tiers.md). The tier chosen in step 1 controls which sections
get asked about, which files get generated, and how many review checkpoints
happen — the criteria and examples in that file are what make tier selection
accurate instead of a guess, so don't skip it.

## The flow

### 1. Pick a tier

Ask the user which tier fits: **Simple**, **Medium**, or **Complex**. Use
tiers.md's "Use when" and "Examples" text to help them decide rather than
just naming the three options — most people don't instinctively know which
bucket their idea falls into. If the user has already described the project
in enough detail to infer the tier confidently, propose one ("this sounds
like a Medium — a few features and a database, but no auth or scale concerns
yet — sound right?") instead of making them choose blind.

The tier is a starting point, not a lock-in. If elicitation answers later
reveal the project is bigger or smaller than expected, say so and propose a
change before continuing — don't silently keep going on a mismatched tier.

### 2. Run elicitation

Open the matching question file: [questions/simple.md](questions/simple.md),
[questions/medium.md](questions/medium.md), or
[questions/complex.md](questions/complex.md) — each is self-contained. Don't
also read medium.md when the tier is Complex; complex.md already folds in
the same topic coverage at its own denser question count.

Ask conversationally, in the stage order the file lays out. Group related
questions into one message rather than firing them one at a time, and skip
anything the user's initial description already answered. All three tiers'
question counts are hard caps, not targets — Simple ≤5, Medium ≤10, Complex
≤20 — ceilings not to cross, not numbers to hit. A shorter pass is always
fine; go over only for a genuine follow-up on a vague answer, never for a
new topic. The caps scale with how much artifact-generation is left to
absorb a long elicitation: Simple's entire deliverable is a single spec.md,
Medium stops at plan.md, and only Complex carries all the way through
tasks.md — so each tier's conversation should stay proportionally tight.

Each question file has stage checkpoints built in (e.g. "show spec.md,
confirm before Stage 2" or, at Complex, per-phase review of tasks.md).
Follow those — don't jump straight from questions to a finished tasks.md
without the review gates in between. The checkpoints are the whole point:
they're where the user catches a wrong assumption before it becomes code.

### 3. Draft spec.md

Read [templates/spec.template.md](templates/spec.template.md). Sections are
tagged with `tier: simple+` / `tier: medium+` / `tier: complex` HTML
comments — include a section if its tag matches the chosen tier or any tier
below it. Fill sections from the elicitation answers, then strip every HTML
comment and tier tag before writing the real file. Follow the template's own
instruction to omit a section entirely (or an empty bullet within one)
rather than padding it with a placeholder — an absent section reads better
than a fake one, and it signals to the coding agent that nothing was decided
there rather than implying something was.

Write the result as `spec.md` **in the user's actual project directory** —
wherever they're working on the real project, never inside this skill's own
folder. Show it to the user and get explicit confirmation or revisions
before moving to the next step.

**At Simple tier, spec.md is the entire deliverable.** Once it's confirmed,
skip straight to step 6 (hand off) — there's no plan.md or tasks.md to
generate. A project small enough to fit in a 5-question elicitation and a
single-file spec doesn't need a separate task breakdown; the spec's
`requirements` section already is the completion bar.

### 4. Draft plan.md — Medium and Complex only

Skip this step entirely at Simple tier; tiers.md explains why (the plan
folds into the task list itself when the project is this small).

Read [templates/plan.template.md](templates/plan.template.md), same
tier-tag rule as spec.md. Write `plan.md` next to `spec.md`. Show it and get
confirmation before moving on. Every plan decision should trace back to
something already in spec.md — if a plan decision doesn't map to a
requirement, constraint, or NFR clause in the spec, that's a sign either the
spec is missing something or the plan is over-building; flag it to the user
rather than silently resolving it either way.

**At Medium tier, plan.md is the final deliverable.** Once it's confirmed,
skip step 5 and go straight to step 6 (hand off) — `tasks.md` is reserved
for Complex, where a phased breakdown actually earns its keep. At Medium,
plan.md's File/Module Structure plus spec.md's requirements are enough for
a coding agent to work from.

### 5. Draft tasks.md — Complex only

Skip this step entirely at Simple and Medium tiers — see the notes at the
end of steps 3 and 4.

Read [templates/tasks.template.md](templates/tasks.template.md). Use the
phased structure, and generate it phase by phase — confirm each phase with
the user before drafting the next one's detail, rather than dumping the
whole task list at once. Complex is the tier where handing a coding agent a
large unreviewed task list is most likely to go sideways.

Every task needs an Acceptance line a coding agent (or a human) can check
without guessing, and a Refs line pointing back to the spec/plan section
that justifies it. A task with no traceable justification is scope creep
that snuck in during drafting, not a real task — cut it or ask.

### 6. Hand off

Once spec.md (and plan.md, tasks.md as applicable) are confirmed, tell the
user the files are ready to hand to a coding agent, and name the actual
file paths. Don't offer to start writing the project's code yourself as part
of this flow — this skill's job ends at the blueprint, not the build.

## Notes

- Output files always go into the user's working project directory, never
  into this skill's own folder. This folder (wherever it's installed) only
  holds the skill's own reference materials — tiers.md, templates/,
  questions/ — which are read from, not written to.
- If the user already has a spec.md/plan.md/tasks.md from a previous run of
  this skill and wants to revise rather than restart, read the existing
  file(s) first and treat the conversation as an editing pass rather than
  running elicitation from scratch.
- If the user's idea is genuinely trivial — something you could write
  correctly in one shot with no real ambiguity — say so and offer to skip
  the process rather than forcing three files onto a five-line script. The
  Simple tier already exists for small projects; some things are smaller
  than even that.
