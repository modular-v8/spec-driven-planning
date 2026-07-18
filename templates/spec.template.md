<!--
TEMPLATE FOR THE AGENT — not the deliverable.

Usage: copy the sections tagged for the chosen tier (and every tier below
it) into the real spec.md, fill them from the elicitation answers, then
strip every HTML comment and tier tag before showing it to the user.

Tier tags:
  tier: simple+   -> include at Simple, Medium, and Complex
  tier: simple    -> Simple tier ONLY (a different, leaner variant exists
                      below tagged tier: medium+ for the same section —
                      use exactly one variant, never both)
  tier: medium+   -> include at Medium and Complex only
  tier: complex   -> include at Complex only

If an answer wasn't collected (question skipped/not applicable), omit the
section rather than filling it with a placeholder — an absent section is
better than a fake one. This applies per-bullet too: don't pad "in scope"
or "requirements" with filler just to look thorough.

Requirements use EARS-style syntax (Easy Approach to Requirements Syntax):
  - Always active   -> "The system SHALL <always-on behavior>."
  - Event-driven     -> "WHEN <trigger>, the system SHALL <response>."
  - Unwanted behavior -> "IF <bad condition>, the system SHALL <specific response>."
Each line should be independently testable. Not every subsection needs
entries at every tier — a small script may have no event-driven behavior
at all, and that's fine; omit the subsection rather than forcing one in.

The `requirements` section has two variants below — Simple tier gets a
flat, unlabeled list (always-active and event-driven statements mixed
together, since a small project doesn't need the categories kept apart);
Medium and Complex keep the three labeled subsections. Both variants keep
`unwanted behavior` as its own subsection.
-->

# spec: {{feature name}}

<!-- tier: simple+ -->
## outcome
One paragraph. What does a user successfully do that they couldn't before?
Should let a stranger understand the point of this without reading anything
else.

<!-- tier: simple+ -->
## in scope
- {{Concrete thing}}
- {{Concrete thing}}

<!-- tier: simple+ -->
## out of scope
- {{Thing explicitly excluded and why}}

<!-- tier: medium+ -->
## users & context
Who uses this, in what environment, how often, and at what technical level.
Keep this brief for internal/personal tools — it exists to shape UX/CLI
decisions later, not to be exhaustive.

<!-- tier: simple+ -->
## constraints
- Stack: {{your actual stack}}
- {{Rule about what not to touch}}
- {{File or pattern to follow}}
- {{Package restrictions}}

<!-- tier: medium+ -->
## data & integrations
External services, APIs, data sources/sinks, and any auth those require. If
something needs to persist, sketch its shape here (full data model detail
belongs in plan.md, not here).

<!-- tier: simple+ -->
## prior decisions
- {{Decision}}: {{Why it was made. Don't suggest alternatives.}}

<!-- tier: simple -->
## requirements
- The system SHALL {{always-on behavior}}.
- WHEN {{trigger}}, the system SHALL {{response}}.

### unwanted behavior
- IF {{bad condition}}, the system SHALL {{specific response}}.

<!-- tier: medium+ -->
## requirements

### always active
- The system SHALL {{always-on behavior}}.

### event-driven
- WHEN {{trigger}}, the system SHALL {{response}}.
- WHEN {{trigger}}, the system SHALL {{response}}.

### unwanted behavior
- IF {{bad condition}}, the system SHALL {{specific response}}.

<!-- tier: complex -->
## risks & open questions
Known unknowns and assumptions that still need validation. Flag anything the
user was unsure about during elicitation rather than silently picking an
answer for them.

<!-- tier: medium+ -->
## acceptance criteria
- [ ] {{Testable pass/fail check}}
- [ ] {{Testable pass/fail check}}
- [ ] {{Edge case check}}
- [ ] {{Constraint validation check}}

Not included at Simple tier — spec.md's requirements section is treated as
the completion bar for something this small; a separate pass/fail list is
redundant overhead. Simple's deliverable ends after `requirements`.
