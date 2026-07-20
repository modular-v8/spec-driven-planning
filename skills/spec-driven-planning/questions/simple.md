# Question Set — Simple Tier

**Target: up to 5 questions, hard cap — not a target to hit, a ceiling not
to cross.** One stage only. spec.md is the *entire* deliverable at this
tier — there's no plan.md and no tasks.md, so elicitation only needs to
cover what spec.md actually holds: outcome, in scope, out of scope,
constraints, prior decisions, and requirements. No acceptance-criteria
question either — Simple's spec.md has no acceptance criteria section, the
requirements themselves are the completion bar.

Ask conversationally, not as a wall of questions. Skip anything the user's
initial request already answered, and don't ask a question whose answer is
obvious from context. Most runs should land at 4 questions; only reach for
a 5th if something genuinely needs a follow-up.

## Spec Elicitation

1. **What can someone do after this exists that they can't do now? What
   are the concrete must-haves?**
   Why: covers both `outcome` and `in scope` in one pass — get a paragraph
   for the former and a bulletable list for the latter.

2. **Anything this should explicitly NOT do?**
   Why: `out of scope`. Even a small script benefits from one boundary
   line — cheapest scope-creep insurance you can buy.

3. **What's the stack, are there any hard rules, and is anything already
   decided that I shouldn't second-guess or suggest alternatives to?**
   (files not to touch, packages not to add, an existing pattern to
   follow, any locked-in decisions)
   Why: `constraints` and `prior decisions` combined — both are "things
   fixed before drafting starts," so they belong in the same question
   rather than costing two of the five slots.

4. **Walk me through it: what should always be true, what triggers what
   response, and what should happen on bad input or errors?**
   Why: `requirements` — one guided question that seeds the whole section.
   At this tier the answer becomes a flat list (no "always active" /
   "event-driven" subheaders splitting it up — see spec.template.md), with
   `unwanted behavior` kept as its own subsection. Don't force all three
   kinds of answer if the project genuinely doesn't have one — a pure
   transform script may have no event-driven behavior at all.

5. *(only if genuinely needed)* One follow-up to pin down a vague answer
   above, or to catch something the initial request implied but never
   spelled out. This is not a mandatory fifth question — most Simple-tier
   runs should finish at 4.

## Before Generating spec.md

Once these are answered, draft spec.md directly — there's no separate
tasks-stage question to ask, since tasks.md doesn't exist at this tier.

## Checkpoint

Show spec.md. Get explicit confirmation (or revisions). This is the tier's
only checkpoint. Once it passes, hand off — spec.md is the finished
deliverable, there's nothing further to generate.
