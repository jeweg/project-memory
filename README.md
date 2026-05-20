# Project Memory

Most serious agent-assisted projects end up needing three things,
informally:

* RULES for how the agent should behave in this codebase: safety
  rules, build commands, debugging conventions, the "do not edit
  g_*" kind of stuff, the local sandboxed-vs-real-hardware truth.
* META: what's happening right now -- hot list, action items, what
  the project IS and where it's going.
* PROJECT-DOMAIN KNOWLEDGE: invariants, decisions, gotchas,
  debugging lore, architecture sketches. The expensive reasoning
  that code alone doesn't reliably surface in a bounded agent
  session.

If you've built agent-assisted projects, you probably already have
your own version of each. A custom AGENTS.md. A PLAN.md or TODO.md.
A personal markdown memory folder. Some project-specific scaffold
doc. They work, and the fact that experienced engineers keep
reaching for these things is a pretty good sign the instinct is
right. What they tend to lack is a shape that ports between projects
or engineers, and a maintenance habit that survives the first quiet
month.

Project Memory is the structured version of that instinct. Three
folders plus a small composition system give each of those three
things a known place. The agent maintains them as you work, so the
structure doesn't rot.

Setup is one folder copy. Works in Cursor / Claude Code / Codex out
of the box, no per-tool config. The next session on the same repo
is never the first session.

## The three places

### Rules: `_agent-rules/`

Numbered markdown fragments get composed into a generated `AGENTS.md`
(and a `CLAUDE.md` shim for Claude Code). The template ships its own
core rules in `_agent-rules/50-project-memory-core.md`, which govern
the discipline below. You add project-specific or team-specific
rules alongside -- `40-team.md`, `60-project.md`, that kind of
thing. The composer concatenates them in numeric order.

If you already have a beloved AGENTS.md for your project, drop it
in as `_agent-rules/40-mine.md`. You get the same content plus the
template's discipline -- nothing is lost.

### Meta: `_knowledge/STATE.md` (always), `_knowledge/IDENTITY.md` and `_knowledge/VISION.md` (optional)

`STATE.md` is the file the agent reads first. It opens with a Hot
List -- the five most important things right now -- then carries
Purpose, Open Questions, Action Items. Rewritten as understanding
matures, not just appended to.

`IDENTITY.md` (introduced when earned) holds the project's
worldview, architectural commitments, posture. The slow-changing
stuff.

`VISION.md` (also when earned) holds long-term ambitions, reframes,
moon shots.

These three together replace the ad-hoc PLAN.md / TODO.md /
DESIGN.md collections that nobody really maintains.

### Project-domain knowledge: `_knowledge/materials/`

Where the actual subject-matter knowledge lives. Source notes,
entity notes, synthesis notes, design docs, debugging summaries,
handoffs for the next agent -- whatever the project accumulates.

Default habit: if something project-related might matter later, tell
the agent in the project. You do not have to decide whether it belongs
in `STATE.md`, a material, the index, or nowhere. The agent routes it:
some context stays in chat, some updates current state, and some
becomes durable project memory.

You can also put files directly in `_knowledge/materials/`. Markdown,
documents, images, exports, binary files, and other source artifacts
are all fine. Human-added files do not need to follow the lowercase
filename convention used for agent-created notes. Once a file is in
`materials/`, it is part of the shared project knowledge namespace:
agents may read it, summarize it, cross-reference it, create an
indexed note about it, or ignore it if it is irrelevant or unsupported.
Agents should edit human-added source artifacts only when asked. If a
file should not be read at all or contains something that should not
live in the repo, keep it somewhere else or say so explicitly.

For files you want in the project corpus, use `_knowledge/materials/`
or ask an agent to summarize the file there. Agents use `materials/`
as the active knowledge namespace. If you only want to retain a raw
source or historical backup, put it in `_knowledge/archive/` instead;
agents may read archive files when source detail is needed, but they
do not scan archive proactively.

`_knowledge/materials/INDEX.md` is the keyword index over them,
updated by the agent as markdown working notes appear.

`_knowledge/materials/OVERVIEW.md` is the external-facing
announcement of what the project knows -- a paragraph plus keywords
that lets another agent or MCP server figure out whether to ask
your project about a topic.

Low ceremony by default. You can let the agent handle the
bookkeeping (INDEX entries, cross-references, freshness markers)
and add structure later if it earns its keep. Most files in a
working `materials/` aren't opened in any given session -- their
job is to exist when needed.

## What you observe after one session

Open the empty template in any of the three tools, tell the agent
what the project is, and ask it to start. In a typical session you
should see:

* `STATE.md` rewritten with real content (purpose, hot list, action
  items).
* `materials/OVERVIEW.md` filled in for external readers.
* The first one or two materials notes capturing whatever you
  discussed.
* `INDEX.md` entries for those notes.

Most of the maintenance happens without you asking -- the template's
core rules tell the agent to do it. The intended posture is to give
the agent useful context and let it keep the knowledge layer coherent,
then review important changes with normal git habits. Compliance
varies a bit across tools and model tiers (the template's instructions
are prose, and how strictly each model follows them isn't fully under
the template's control), but in my testing the pattern shows up
reliably within the first session or two. You can also drive
maintenance explicitly when useful -- "consolidate these materials
into STATE.md", "audit state", "wrap up the session and summarize
what changed".

Across sessions and across machines: the next agent opens the same
repo, reads `STATE.md`, sees the hot list, knows what's going on.
The reasoning doesn't have to be reconstructed from chat history
that lives somewhere else.

## What this is not

* Not a service. No backend, no hosted index, no SaaS to subscribe
  to.
* Not a vector DB or graph store. At project scale, grep over
  markdown is fast, inspectable, and sufficient -- heavier
  retrieval engines would add deployment cost and indirection
  without earning it. (When a project genuinely outgrows grep, swap
  in a heavier engine on the same markdown substrate; the corpus
  stays portable.)
* Not tied to one tool. Works wherever `AGENTS.md` or `CLAUDE.md`
  is read.
* Not heavy ceremony. You can ignore most of it; the agent
  maintains it.
* Not a replacement for your code, your bug tracker, your build
  system. It sits alongside them and holds what they don't.

## Setup

1. Copy this folder to your project location (or `git clone` and
   delete `.git/`).
2. Delete this README; it's template setup material, not project
   content.
3. Open the folder in your AI tool of choice. Tell the agent what
   you want to do -- e.g. "let's start this project, it'll be about
   ...".
4. The template-default banners in `STATE.md` and `OVERVIEW.md`
   steer the agent into the bootstrap.

That's it. No install step, no service, no per-tool configuration.

## Customizing the agent rules

Whether your tool reads `AGENTS.md` (Cursor, Codex CLI / IDE) or
`CLAUDE.md` (Claude Code), the template generates both from the same
source fragments under `_agent-rules/`. To add or change rules:

1. Edit `_agent-rules/*.md`. Add new fragments like `40-team.md` for
   team conventions or `60-project.md` for project-specific rules.
   Numeric prefix sets order (lower comes first).
2. Run `python _agent-rules/compose.py` to regenerate `AGENTS.md`
   and the `CLAUDE.md` shim.
3. The composer refuses to overwrite `AGENTS.md` if it detects
   changes it didn't write, so a hand-edit (yours or an agent's)
   won't get silently clobbered. Copy any such changes into
   `_agent-rules/*.md` and re-run, or pass `--force`.

Check for stale output:

```powershell
python _agent-rules/compose.py --check
```

## Design rationale

Design decisions, alternatives considered, and the changelog of
breaking changes live in the separate meta repo
*project-memory-meta*. Read it when extending the template,
migrating an existing project to a newer template version, or just
curious about the why.
