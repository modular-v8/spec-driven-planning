# Question Set — Medium Tier

**10 questions total, hard cap — not a target to hit, a ceiling not to
cross.** Two stages (spec, then plan) — no tasks stage. `tasks.md` is
reserved for Complex tier; at Medium, plan.md's File/Module Structure plus
spec.md's requirements are what the coding agent works from directly.
Confirm spec.md with the user before opening the plan stage — plan
questions rely on the spec being settled. Skip anything already answered,
and combine related questions into one conversational turn where it reads
naturally — most runs should land under 10, not exactly at it.

## Stage 1 — Spec Elicitation (5 max)

1. **What's this project, who's it for (technical level, environment, how
   often they'll use it), and what can they do after it exists that they
   can't now?**
   Why: `outcome` + `users & context` in one combined question.

2. **What are the concrete must-have behaviors, and is there anything this
   should explicitly NOT do for this version (and why)?**
   Why: `in scope` + `out of scope` combined — asking the boundary right
   alongside the must-haves keeps both fresh in the same breath.

3. **What's the stack, and are there hard rules?** (files/patterns not to
   touch, package restrictions) **Anything already decided that I
   shouldn't relitigate or suggest alternatives to?**
   Why: `constraints` + `prior decisions` combined — both are "things
   fixed before the plan stage starts."

4. **Any external integrations or data sources involved? Does anything
   need to persist, and roughly what shape?**
   Why: `data & integrations`. Full data-model detail belongs in plan.md
   — this just seeds it.

5. **Walk me through the requirements: what should always be true, what
   are the trigger→response behaviors, what should happen on bad input or
   errors, and how will you validate it's complete and correct when
   done?**
   Why: `requirements` (always active / event-driven / unwanted behavior)
   plus `acceptance criteria` in one guided question — push for specifics
   ("I can log in and see my dashboard" beats "it works").

## Checkpoint

Show spec.md. Get explicit confirmation (or revisions) before Stage 2.

## Stage 2 — Plan Elicitation (5 max)

6. **Is this greenfield or does it need to follow an existing
   architecture/pattern? Any tech stack still open for this stage to
   decide, or libraries you specifically want to avoid? Any folder
   structure conventions you already use?**
   Why: `Approach Summary` / `Architecture` / `Tech Stack & Key
   Decisions` / `File-Module Structure` — one combined question covering
   plan.md's structural sections.

7. **Is this a one-time build, or does it need to be maintained/extended
   later?**
   Why: calibrates how much abstraction/structure is warranted — the
   single highest-leverage plan question for avoiding both over- and
   under-engineering.

8. **How should errors and edge cases surface?** (logs, UI messages,
   thrown exceptions, silent retries...)
   Why: feeds directly into the coding agent's implementation — this is
   the closest Medium gets to an error-handling spec, since there's no
   tasks.md acceptance line to carry it instead.

9. **Any performance expectations, and what are your testing
   expectations?** (data volume, concurrency, response time; none/manual
   click-through/automated tests)
   Why: `performance` + `testing expectations` combined — one gut-check
   before a coding agent picks a naive approach, plus what "done" requires
   on the testing front, since there's no tasks.md to carry it as a
   checklist item.

10. *(optional)* **Any part of this you'd rather review/build
    incrementally instead of all at once?**
    Why: last chance to insert an extra checkpoint before full handoff to
    the coding agent — worth asking since Medium has no tasks.md stage
    left to catch this later. Skip if the project is small enough that
    a single handoff is obviously fine.

## Checkpoint

Show plan.md. Get explicit confirmation (or revisions). This is Medium
tier's final checkpoint — once plan.md is confirmed, hand off directly.
There's no tasks.md stage.
