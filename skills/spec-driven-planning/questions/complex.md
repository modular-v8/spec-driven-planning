# Question Set — Complex Tier

**20 questions total, hard cap — not a target to hit, a ceiling not to
cross.** Three stages (spec, plan, tasks). This file is self-contained —
don't also run medium.md's questions; the topics medium.md covers are
folded into the questions below. This is the tier where following up on
vague answers matters most — if the user says "needs to scale" or "handle
sensitive data," don't move on until you have a number or a concrete
requirement. A vague requirement here is exactly what causes an over- or
under-built system three weeks later. Use any headroom under the cap for
that follow-up probing rather than new topics.

## Stage 1 — Spec Elicitation (8 max)

1. **What's this project, who's it for (technical level, environment, how
   often they'll use it), and what can they do after it exists that they
   can't now?**
   Why: `outcome` + `users & context`.

2. **What are the concrete must-have behaviors, and is there anything this
   should explicitly NOT do for this version (and why)?**
   Why: `in scope` + `out of scope`.

3. **What's the stack, are there hard rules, and is anything already
   decided that I shouldn't relitigate or suggest alternatives to?**
   Why: `constraints` + `prior decisions`.

4. **Are there stakeholders beyond the primary user who need to sign off
   or are otherwise affected?** (legal, security, other teams, customers)
   **Any compliance, security, or privacy requirements?** (data
   residency, PII handling, audit logging, industry regulations)
   Why: often surfaces requirements the primary user wouldn't think to
   mention unprompted; feeds `constraints` and `requirements`.

5. **Any external integrations or data sources? Does anything need to
   persist, and roughly what shape?**
   Why: `data & integrations`. Full data-model detail belongs in plan.md
   — this just seeds it.

6. **Is this replacing or migrating from an existing system?** If so,
   what compatibility or migration guarantees are needed. **Are there
   multiple environments to plan for** (dev/staging/prod, region-specific
   deployments)?
   Why: surfaces a category of requirements (data migration, parallel-run
   periods, cutover plan) that's easy to miss otherwise.

7. **Walk me through the requirements: what should always be true, what
   are the trigger→response behaviors, what should happen on bad input or
   errors — and how will you validate it's complete and correct when
   done?**
   Why: `requirements` (always active / event-driven / unwanted behavior)
   + `acceptance criteria` — push for specifics.

8. **What scale is this expected to handle, and what are the
   uptime/latency expectations?** Push for numbers: users, requests/sec,
   data volume, growth rate. Also: **what do you already know is risky or
   unresolved about this project, any budget/infrastructure ceilings, and
   is this a single release or a phased rollout** (if phased, what defines
   each phase)?
   Why: scale/uptime become numeric `requirements` clauses (e.g. "the
   system SHALL respond within 200ms") rather than a vague NFR aside;
   risks feed `risks & open questions`; phasing seeds Sequencing &
   Milestones in plan.md. Ask directly rather than hoping it surfaces
   organically — users often have a nagging worry they haven't voiced.

## Checkpoint

Show spec.md. Confirm before Stage 2. At this tier, read back anything you
inferred rather than asked directly, and let the user correct it.

## Stage 2 — Plan Elicitation (7 max)

9. **Is this greenfield or does it need to follow an existing
   architecture/pattern? Tech stack still open, libraries to avoid, folder
   structure conventions — and should this be split into
   independently-buildable services/modules?** If so, what are the
   natural boundaries?
   Why: Approach / Architecture / Tech Stack / File-Module Structure, plus
   whether tasks.md can be parallelized.

10. **Is this a one-time build, or does it need to be maintained/extended
    later?**
    Why: calibrates how much abstraction/structure is warranted — the
    single highest-leverage plan question for avoiding both over- and
    under-engineering.

11. **How should errors and edge cases surface?** (logs, UI messages,
    thrown exceptions, silent retries...)
    Why: Error Handling Strategy.

12. **What's the auth/authz model, and what observability is needed?**
    (roles/SSO requirements; logging, monitoring, alerting, what "on
    fire" looks like and who gets paged)
    Why: Security & Error Handling Strategy — both often skipped, both
    expensive to add later.

13. **Any performance expectations, and what are your testing
    expectations?** (data volume, concurrency, response time; none/manual
    click-through/automated tests)
    Why: one gut-check before a coding agent picks a naive approach, plus
    what "done" requires on the testing front.

14. **What third-party services does this depend on and what's the
    fallback if one is unavailable? What's the deployment target and are
    there CI/CD expectations?**
    Why: Integration Plan — also determines what needs a mock/stub for
    local development.

15. **Were there alternative approaches you considered and ruled out? Is
    there a data migration/seeding strategy, and if a milestone fails or
    is delayed, what's the fallback?**
    Why: Alternatives Considered + Sequencing & Milestones — prevents the
    coding agent (or a future contributor) from re-opening a settled
    debate, and surfaces which phases are truly load-bearing.

## Checkpoint

Show plan.md. Confirm before Stage 3.

## Stage 3 — Tasks Elicitation (3 max)

16. **Should tasks be organized into phases/milestones with a checkpoint
    between each? Which phase is riskiest and should be tackled or
    validated first?**
    Why: default yes on phasing at this tier unless the user prefers a
    flat list; surfaces sequencing that de-risks the project early.

17. **Are there tasks that explicitly block other tasks I should call out
    as dependencies? Do you want to review/approve each phase before I
    generate the next phase's detail, or generate the full tasks.md
    upfront?**
    Why: feeds the "Depends on" field in tasks.md, and sets the core
    Complex-tier checkpoint behavior — default to phase-by-phase review
    unless the user opts out.

18. **Should tasks explicitly include verification/testing steps, or
    keep those bundled into each task's acceptance line?**
    Why: determines tasks.md's granularity — worth its own question at
    this tier since tasks.md is the deliverable a coding agent executes
    most literally.

## Checkpoint (repeats per phase if phase-by-phase review was chosen)

Show each phase's tasks. Confirm before generating the next phase.

---

18 questions above are the baseline. The remaining headroom to the 20 cap
is for follow-up probing on vague answers (see the note at the top) — not
for new topics.
