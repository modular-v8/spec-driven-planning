# spec-driven-planning

A portable planning skill for AI coding agents. It turns a rough project idea into reviewed planning
documents — `spec.md`, `plan.md`, `tasks.md` — **before** a coding agent writes a single line of
code. It ships in [Claude Code](https://claude.com/claude-code)'s native skill format, but everything
in this repo is plain markdown with no proprietary lock-in — see [Installation](#installation) below
for Cursor, Codex, and any other agent.

## Why

One-shot prompting a coding agent on anything beyond a trivial task tends to produce sprawling,
inconsistent output: the agent infers scope, architecture, and "done" as it goes, with no chance
for a human to correct course until the code already exists. This skill runs a short, conversational
elicitation up front and produces reviewed blueprints instead — so the coding agent (Claude Code,
Cursor, or anything else you hand the files to) starts from a human-confirmed plan rather than a
single freeform prompt.

## How it works

The skill scales itself to the size of the project via three complexity tiers, so a five-minute
script doesn't get the same treatment as a production system:

| | Simple | Medium | Complex |
|---|---|---|---|
| **Use for** | a single-purpose script or tool, one component, no real architecture decision | a multi-file feature or small app, some real design decisions | a multi-component system, auth/persistence/scale concerns, production-bound |
| `spec.md` | light, no acceptance criteria | full | full |
| `plan.md` | — | moderate | deep |
| `tasks.md` | — | — | phased |
| Questions | ≤5 (hard cap) | ≤10 (hard cap) | ≤20 (hard cap) |
| Checkpoints | 1 | 2 | 3+ |

Every question count above is a **ceiling, not a target** — the skill is instructed to ask fewer
questions whenever the answers are already obvious from context, and never pad an elicitation just
to hit a number.

The full flow, tier by tier, is documented in [tiers.md](tiers.md).

## Installation

This skill only needs four things to run: `SKILL.md`, `tiers.md`, `templates/`, and `questions/`.
(`evals/` is a testing harness for skill maintainers, not needed at runtime.)

Across every platform below, the skill is **explicit-invocation only** by design — it's written to
not trigger itself from general planning language like "help me plan this out," so it won't fire
unexpectedly mid-conversation. You have to ask for it by name.

### Claude Code

Global install (available in every project):

macOS / Linux:
```bash
git clone <this-repo-url> spec-driven-planning
mkdir -p ~/.claude/skills/spec-driven-planning
cp -r spec-driven-planning/{SKILL.md,tiers.md,templates,questions} ~/.claude/skills/spec-driven-planning/
```

Windows (PowerShell):
```powershell
git clone <this-repo-url> spec-driven-planning
New-Item -ItemType Directory -Force "$HOME\.claude\skills\spec-driven-planning"
Copy-Item spec-driven-planning\SKILL.md, spec-driven-planning\tiers.md, spec-driven-planning\templates, spec-driven-planning\questions -Destination "$HOME\.claude\skills\spec-driven-planning\" -Recurse
```

Project-local install (only available in one project): copy the same four items into
`<your-project>/.claude/skills/spec-driven-planning/` instead of the home-directory path above.

**Invoke it** by typing `/spec-driven-planning`, or by explicitly saying "use the
spec-driven-planning skill."

### Cursor

Cursor discovers custom instructions through [Project Rules](https://docs.cursor.com/context/rules)
— `.mdc` files under `.cursor/rules/`. Clone this repo, then copy the whole folder into your
project's rules directory, e.g. `.cursor/rules/spec-driven-planning/`, and add a small rule file
(`.cursor/rules/spec-driven-planning.mdc`) that points at it:

```markdown
---
description: Spec-driven planning — turns a rough project idea into reviewed spec.md/plan.md/tasks.md before any code is written. Only apply when the user explicitly asks for it by name.
alwaysApply: false
---
Read `.cursor/rules/spec-driven-planning/SKILL.md` and follow it exactly, starting from "The flow."
Read `tiers.md` and the relevant `questions/*.md` / `templates/*.md` files in that same folder as
each step calls for them.
```

Setting the rule type to "Agent Requested" (not "Always") keeps it explicit-invocation, matching how
the skill is designed to behave. Cursor's rule UI has changed across versions, so check
[Cursor's docs](https://docs.cursor.com/context/rules) if the exact frontmatter differs from what's
above. **Invoke it** by referencing the rule (e.g. `@spec-driven-planning`) or just asking to use it
by name.

### Codex CLI

Codex CLI supports custom slash-command prompts as markdown files under `~/.codex/prompts/` (global)
or `.codex/prompts/` (project-local) — see
[OpenAI's Codex docs](https://developers.openai.com/codex/) for the current mechanism, as this has
moved around between releases. Clone this repo, then either:

- copy the whole folder in as `~/.codex/prompts/spec-driven-planning/` and create a thin
  `~/.codex/prompts/spec-driven-planning.md` entry prompt that tells Codex to read `SKILL.md` in that
  folder and follow it (same pattern as the Cursor rule above), or
- if your Codex version only supports a single prompt file per command, paste `SKILL.md`'s content
  directly into the prompt file, and keep `tiers.md` / `templates/` / `questions/` alongside it in the
  same folder so Codex can read them by relative path as the flow calls for them.

**Invoke it** with the resulting slash command, e.g. `/spec-driven-planning`.

### Any other agent

Nothing here is proprietary — it's markdown that any agent capable of reading files can follow.
Clone this repo somewhere on disk (or into your project), then explicitly tell your agent something
like: *"Read SKILL.md in `<path-to-this-repo>` and follow it — I want to plan out `<your project
idea>`."* The agent will pull in `tiers.md`, the right `questions/*.md`, and the right
`templates/*.md` as it goes.

## Usage

Once invoked (see the platform-specific instructions above), the flow is the same everywhere: pick a
tier → answer that tier's elicitation questions conversationally → review `spec.md` (and `plan.md` /
`tasks.md`, depending on tier) → confirm each file before moving to the next → hand off the finished
files for you to give to a coding agent.

Output files are always written into **your project's working directory**, never into the skill's
own folder.

## Repository structure

```
SKILL.md                  Orchestration logic — read first, drives the whole flow
tiers.md                  Defines the Simple / Medium / Complex tiers and what each controls
templates/
  spec.template.md        spec.md structure, tier-tagged
  plan.template.md        plan.md structure (Medium/Complex only)
  tasks.template.md       tasks.md structure (Complex only)
questions/
  simple.md                elicitation questions, Simple tier
  medium.md                 elicitation questions, Medium tier
  complex.md                 elicitation questions, Complex tier
evals/                    Eval harness data — for skill maintainers testing changes, not runtime
```

## Customizing

- **Tier boundaries and question caps** — edit [tiers.md](tiers.md).
- **What gets asked, and in what order** — edit the relevant file in [questions/](questions/).
- **The shape of the output documents** — edit the relevant file in [templates/](templates/); each
  template uses `<!-- tier: ... -->` HTML comments to mark which tier(s) a section applies to.
