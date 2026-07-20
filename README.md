# spec-driven-planning

A portable planning skill for AI coding agents. It turns a rough project idea into reviewed planning
documents — `spec.md`, `plan.md`, `tasks.md` — **before** a coding agent writes a single line of
code. It ships in [Claude Code](https://claude.com/claude-code)'s native skill format, but everything
in this repo is plain markdown with no proprietary lock-in — see [Installation](#installation) below
for Cursor, Codex, and any other agent.

## Why

Single-shot prompting a coding agent on anything beyond a trivial task tends to produce sprawling,
inconsistent output: the agent infers scope, architecture, and "done" as it goes, with no chance
for a human to correct course until the code already exists. This skill runs a short, conversational
elicitation up front and produces reviewed blueprints instead — so the coding agent (Claude Code,
Cursor, or anything else you hand the files to) starts from a human-confirmed plan rather than a
single freeform prompt.

## What this skill does

It creates `spec.md` (and `plan.md` / `tasks.md`, depending on complexity) in your project, based on
a short question-and-answer conversation about what you're building.

## What this skill does NOT do

It does not write or execute any code. Producing the planning documents is the entire job — once
they're confirmed, this skill's involvement ends. You still need to hand those files to a coding
agent yourself, with a structured prompt that tells it to read them before it starts implementing.

### Handing off to your coding agent

Once `spec.md` (and `plan.md` / `tasks.md`, if generated) are ready, start your coding agent session
with a prompt like this — adjust the conditional lines depending on which files your tier actually
produced:

```
Before writing any code, do the following:

1. Read spec.md to understand the project.
2. If plan.md exists, read it next to understand the overall architecture and approach.
3. If tasks.md exists, read it and note the specific tasks you'll need to complete.

Then summarize your understanding of the project in a few sentences and ask me to confirm it's
correct before you start implementation.
```

This forces the same "confirm before building" discipline on the build side that spec-driven-planning
enforces on the planning side — the coding agent states its understanding back to you before writing
a single line, so a misread spec gets caught immediately instead of three files deep into the
implementation.


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

The full flow, tier by tier, is documented in
[tiers.md](skills/spec-driven-planning/tiers.md).

## Installation

This skill only needs four things to run: `SKILL.md`, `tiers.md`, `templates/`, and `questions/` —
all under [skills/spec-driven-planning/](skills/spec-driven-planning/) in this repo.

Across every platform below, the skill is **explicit-invocation only** by design — it's written to
not trigger itself from general planning language like "help me plan this out," so it won't fire
unexpectedly mid-conversation. You have to ask for it by name.

### Quick install (recommended)

This repo is laid out to be discoverable by
[`npx skills`](https://github.com/vercel-labs/skills) — a universal installer for agent skills that
supports Claude Code, Cursor, GitHub Copilot, the Codex family, OpenCode, Cline, and 70+ other
agents through one command:

```bash
npx skills add modular-v8/spec-driven-planning
```

It auto-detects which coding agents you have installed and prompts you for install targets, then
symlinks (or, with `--copy`, copies) `skills/spec-driven-planning/` into each one's own skills
directory (`.claude/skills/`, `.agents/skills/`, `~/.cursor/skills/`, `~/.copilot/skills/`, etc). No
manual path-juggling required — see the [full CLI reference](https://github.com/vercel-labs/skills)
for flags like `-a` (target a specific agent) or `-y` (non-interactive).

### Manual installation

Only needed if you'd rather not run `npx`/Node, or your agent isn't covered by the CLI above. Clone
the repo first:

```bash
git clone https://github.com/modular-v8/spec-driven-planning.git
```

Every path below is relative to the cloned repo, and always points at the
`skills/spec-driven-planning/` subfolder specifically — not the repo root.

#### Claude Code

Global install (available in every project):

macOS / Linux:
```bash
mkdir -p ~/.claude/skills/spec-driven-planning
cp -r spec-driven-planning/skills/spec-driven-planning/* ~/.claude/skills/spec-driven-planning/
```

Windows (PowerShell):
```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\skills\spec-driven-planning"
Copy-Item spec-driven-planning\skills\spec-driven-planning\* -Destination "$HOME\.claude\skills\spec-driven-planning\" -Recurse
```

Project-local install (only available in one project): copy the same folder into
`<your-project>/.claude/skills/spec-driven-planning/` instead of the home-directory path above.

**Invoke it** by typing `/spec-driven-planning`, or by explicitly saying "use the
spec-driven-planning skill."

#### Cursor

Newer versions of Cursor read skills natively from a shared `.agents/skills/<name>/` folder
(project) or `~/.cursor/skills/<name>/` (global) — the same layout `npx skills` installs into — so
copying `skills/spec-driven-planning/` straight into one of those paths may just work. Check
[Cursor's docs](https://docs.cursor.com/context/rules) for current support.

If your version doesn't support that yet, fall back to
[Project Rules](https://docs.cursor.com/context/rules) — `.mdc` files under `.cursor/rules/`. Copy
`skills/spec-driven-planning/` into `.cursor/rules/spec-driven-planning/`, and add a small rule file
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
the skill is designed to behave. **Invoke it** by referencing the rule (e.g. `@spec-driven-planning`)
or just asking to use it by name.

#### Codex CLI

Codex CLI supports custom slash-command prompts as markdown files under `~/.codex/prompts/` (global)
or `.codex/prompts/` (project-local) — see
[OpenAI's Codex docs](https://developers.openai.com/codex/) for the current mechanism, as this has
moved around between releases. Either:

- copy `skills/spec-driven-planning/` in as `~/.codex/prompts/spec-driven-planning/` and create a
  thin `~/.codex/prompts/spec-driven-planning.md` entry prompt that tells Codex to read `SKILL.md` in
  that folder and follow it (same pattern as the Cursor rule above), or
- if your Codex version only supports a single prompt file per command, paste `SKILL.md`'s content
  directly into the prompt file, and keep `tiers.md` / `templates/` / `questions/` alongside it in the
  same folder so Codex can read them by relative path as the flow calls for them.

**Invoke it** with the resulting slash command, e.g. `/spec-driven-planning`.

#### GitHub Copilot

Newer Copilot builds may support the same shared `.agents/skills/<name>/` folder `npx skills`
installs into (`~/.copilot/skills/<name>/` globally) — check
[GitHub's Copilot customization docs](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions)
for current support before falling back to the manual route below.

Otherwise, Copilot Chat (in VS Code) supports reusable **prompt files** — markdown files under
`.github/prompts/` invoked with a `/` slash command, which stays explicit-invocation rather than
always-on. (Skip `.github/copilot-instructions.md` for this skill — that route applies to every chat
request automatically, which conflicts with the explicit-invocation design.)

Copy `skills/spec-driven-planning/` into `.github/prompts/spec-driven-planning/`, and add a thin
entry prompt at `.github/prompts/spec-driven-planning.prompt.md`:

```markdown
---
mode: agent
description: Spec-driven planning — turns a rough project idea into reviewed spec.md/plan.md/tasks.md before any code is written.
---
Read `.github/prompts/spec-driven-planning/SKILL.md` and follow it exactly, starting from "The flow."
Read `tiers.md` and the relevant `questions/*.md` / `templates/*.md` files in that same folder as
each step calls for them.
```

Prompt files are a fairly recent VS Code feature and their exact frontmatter/location has shifted
between releases, so double-check against current docs if what's above doesn't match your version.
**Invoke it** by typing `/spec-driven-planning` in Copilot Chat.

#### Any other agent

Nothing here is proprietary — it's markdown that any agent capable of reading files can follow. Copy
`skills/spec-driven-planning/` somewhere on disk (or into your project), then explicitly tell your
agent something like: *"Read SKILL.md in `<path-to-that-folder>` and follow it — I want to plan out
`<your project idea>`."* The agent will pull in `tiers.md`, the right `questions/*.md`, and the right
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
skills/spec-driven-planning/    The installable payload — this is what npx skills / manual install copies
  SKILL.md                      Orchestration logic — read first, drives the whole flow
  tiers.md                      Defines the Simple / Medium / Complex tiers and what each controls
  templates/
    spec.template.md            spec.md structure, tier-tagged
    plan.template.md            plan.md structure (Medium/Complex only)
    tasks.template.md           tasks.md structure (Complex only)
  questions/
    simple.md                   elicitation questions, Simple tier
    medium.md                   elicitation questions, Medium tier
    complex.md                  elicitation questions, Complex tier
README.md, LICENSE              Repo-level docs — not part of the installed skill
```

## Customizing

- **Tier boundaries and question caps** — edit
  [tiers.md](skills/spec-driven-planning/tiers.md).
- **What gets asked, and in what order** — edit the relevant file in
  [questions/](skills/spec-driven-planning/questions/).
- **The shape of the output documents** — edit the relevant file in
  [templates/](skills/spec-driven-planning/templates/); each template uses `<!-- tier: ... -->` HTML
  comments to mark which tier(s) a section applies to.
