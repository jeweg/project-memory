# Project Memory Core Rules

## Purpose

This workspace is organized to preserve project knowledge across short-lived
agent sessions.

The agent's primary job is to keep useful understanding from living only in
chat. Capture findings, maintain the current state, and keep the materials
index discoverable.

## Non-Negotiable Invariants

* Read `_knowledge/STATE.md` first for non-trivial work.
* Capture durable findings in the same turn; do not leave them only in chat.
* Treat substantial handoffs, transcripts, email threads, and design notes as
  durable context; summarize them into `materials/`, not only chat.
* Store project memory in `_knowledge/`, not in tool-private memory stores.
* Treat `STATE.md`, `IDENTITY.md`, and `VISION.md` as authoritative over
  materials unless the user says otherwise.
* Update `INDEX.md` whenever markdown working materials are created, renamed,
  moved, or substantially updated.
* Update `OVERVIEW.md` when the project's externally visible scope changes.
* Do not create `IDENTITY.md` or `VISION.md` eagerly.
* Do not move files into `_knowledge/archive/` unless the user asks; list
  archive candidates instead.
* Keep audit, consolidation, and mechanical validation distinct.

## Workspace Structure

Project memory lives under `_knowledge/` at the workspace root. It has two
layers with different audiences:

* Curated layer: `_knowledge/STATE.md`, plus, when the project has earned them:
  `_knowledge/IDENTITY.md` and `_knowledge/VISION.md`. These files are the
  project's internal scaffolding: current state, posture, commitments, and
  ambition. They are not external-facing.
* Materials layer: `_knowledge/materials/`. This is the project's knowledge
  namespace: notes, explorations, session bridges, source summaries, and, for
  knowledge-heavy projects, durable reference artifacts that may be the
  project's deliverable.

Product code, scripts, documents, or other project deliverables sit beside
`_knowledge/` in whatever shape the project needs. In knowledge-heavy projects,
the deliverable may itself live under `_knowledge/materials/`; that is the
design point, not a structural violation.

### Curated Layer

* `_knowledge/STATE.md` is always present. It records what is happening now:
  hot list, in-flight work, unresolved questions, action items, and decisions
  whose rationale is still being absorbed. Rewrite sections as understanding
  matures; do not append stale update trails.
* `_knowledge/IDENTITY.md` is optional. It records what the project is:
  purpose, mental model, architecture, posture, conventions, and constraints
  that should not be renegotiated casually.
* `_knowledge/VISION.md` is optional. It records where the project is heading:
  long-term capabilities, reframes, moon shots, and ambitions worth preserving
  without forcing them into the active roadmap.

Do not create `IDENTITY.md` or `VISION.md` eagerly, but do not defer forever.
When stable purpose, architecture, posture, conventions, constraints, or
non-goals have accumulated in `STATE.md` or materials, say the project has
earned `IDENTITY.md`, name what should move, and create it once the user
approves or asks to consolidate, promote, or factor the knowledge layer (unless
the task is report-only). Keep `VISION.md` rarer: recommend or create it only
for durable long-term direction, usually after `IDENTITY.md` exists.

If `VISION.md` exists without `IDENTITY.md`, that is a smell. Vision without a
grounded identity tends to drift; introduce `IDENTITY.md` first.

Each curated file should open with a short preamble pointing at the other
curated files that exist. `STATE.md` points at `IDENTITY.md`, `VISION.md`, and
any canonical design material when present. `IDENTITY.md` and `VISION.md` point
back to `STATE.md` and to each other when both exist.

### Materials Layer

`_knowledge/materials/` contains three kinds of files:

* Markdown working files: agent-created lowercase files such as
  `topic-description.md`. They hold exploration, session notes, partial
  findings, source summaries, and durable knowledge artifacts. They may be
  messy, redundant, contradictory, or stale. Err on the side of capturing; lost
  knowledge is more expensive than a messy file.
* Human-added context and source files: files placed directly in `materials/`
  by a user. They may use any useful filename or file type, including
  non-Markdown and binary formats. Do not rename them merely to satisfy the
  agent-created markdown naming convention. Use what you can read, summarize or
  cross-reference what is useful, and ignore what is irrelevant or unsupported.
  Edit these source artifacts only when the user asks.
* `INDEX.md` and `OVERVIEW.md`: the two canonical files that make the
  namespace discoverable.

`_knowledge/materials/INDEX.md` is the internal keyword index for lowercase
markdown working files. It is how agents find relevant notes without reading
the whole directory. A markdown working file without an index entry is
invisible. For human-added source files, including non-lowercase Markdown,
non-Markdown, and binary files, create or update an indexed markdown note that
points at them when they need to be discoverable.

`_knowledge/materials/OVERVIEW.md` is the project discovery profile for
external readers and routing agents. It should read like a compact landing
README for the project as represented by the materials: what the project is,
what it is making or investigating, what it may be aiming to become, and when
another agent should query this project as a knowledge base. It must include a
separate one-sentence `Summary` section for dashboards and project lists, a
longer `Description`, broad `Keywords`, and an agent-facing `Can Answer`
section. It should not lead with "this materials namespace contains..." or
become a file-by-file inventory. Mention specific materials only when a
reference helps a routing agent decide whether to query this project. It
describes externally useful project knowledge, not `STATE.md`, `IDENTITY.md`,
`VISION.md`, or internal operational status. It is an announcement, not a
license to expose every material externally.

Do not infer a canonical project name from the directory name. In `Summary`,
prefer a name-free capability or scope sentence. Use a project, product, or
package name only when it is canonical in project context, such as README title,
package metadata, CLI name, or user-provided identity.

`_knowledge/archive/` holds materials whose insights have been fully absorbed,
plus raw sources that back summaries in `materials/`. Agents may read archive
files when source detail is needed. Agents do not move files into archive unless
the user explicitly asks; normally, surface the list of fully absorbed files and
let the user move them.

### Template Setup Material

If a top-level `README.md` still contains the Project Memory template setup
instructions, treat it as setup material, not project content. Ignore it during
normal work. Once the project is bootstrapped, suggest deleting the setup
README; do not delete a README that has become real project documentation.

### Implementation Layout

Implementation directories are project-specific. Layout, run commands, test
commands, and key conventions belong in `STATE.md` or, when stable and
worldview-shaped, `IDENTITY.md`.

If a project has substantial implementation but no orientation pointers in the
knowledge layer, add a brief `## Layout` section to `STATE.md` naming the main
directories and commands.

## Reading Order

At the start of a session, read the cheapest sufficient context:

1. Read `_knowledge/STATE.md` first. Its two opening sections are the cheapest
   entry point: `Hot List` for what is live, `Where To Look` for where the rest
   of the corpus is.
2. Read `_knowledge/IDENTITY.md` when the task touches architecture, posture,
   conventions, constraints, or when you are unsure whether a shortcut matches
   the project's worldview.
3. Read `_knowledge/VISION.md` for prioritization, reflection, big-picture
   direction, or long-term planning.
4. Read `_knowledge/materials/INDEX.md` when you need supporting context from
   materials, then open only the relevant materials.

Do not read all knowledge files for every trivial task.

## Core Operating Rules

Maintain knowledge continuously, not as a cleanup phase:

* Update the curated layer when work produces current operational state,
  project commitments, or long-term direction.
* Capture working knowledge in `materials/` when it is preliminary, detailed,
  source-like, or useful for a future agent to rediscover.
* Keep `INDEX.md` current whenever markdown working-material membership or
  substantive content changes.
* Keep `OVERVIEW.md` current when the project's externally visible scope
  changes.

Knowledge coherence is part of normal edits. When you touch a curated file or a
materials file, re-read the sections and cross-references you touched. Fix
obvious bookkeeping drift directly: stale index entries, moved-file references,
or freshness markers that contradict the edit. If the fix would require
interpreting meaning, flag it instead of guessing.

## Routing Findings

When work produces a finding, decide where it belongs:

* Put current, operational, in-flight, or soon-to-change knowledge in
  `_knowledge/STATE.md`.
* Put stable purpose, architecture, posture, or constraint knowledge in
  `_knowledge/IDENTITY.md` when it exists. If it clearly belongs there but the
  file does not exist yet, follow the introduction rule above: stage it in
  `STATE.md` during ordinary work, recommend the lift once the project has
  earned it, and create it when authorized.
* Put long-term ambition, future direction, or moon-shot framing in
  `_knowledge/VISION.md` when it exists. If it does not exist yet, usually
  capture the material in `materials/`; create it only when authorized and the
  durable long-term direction is clear.
* Put preliminary findings, source summaries, session bridges, handoffs, email
  threads, transcripts, design notes, and detailed reference material in
  `_knowledge/materials/`.

A finding can have multiple aspects. Split operational, architectural, and
long-term parts across the appropriate homes and cross-reference them rather
than forcing everything into one file.

Capture in the same turn as you produce the finding. Do not present analysis
and ask "shall I write this up?" Write the appropriate file and announce what
you changed. Objection is cheaper than missed capture.

Substantial pasted context is durable by default. When the user gives a long
handoff, email thread, meeting transcript, design note, or similar source, write
a materials summary unless the user explicitly asks you not to.

Users do not need to pre-classify every piece of project context. If a user
tells you something project-related, decide whether it is ephemeral chat
context, current state, a durable material, or an index/update task, then route
it accordingly.

Inside `_knowledge/materials/`, create files freely when you have durable
context to preserve. Outside `materials/`, update existing curated files as the
rules require, but do not create or move files unless the user explicitly asks
or a specific rule here names that action. The `IDENTITY.md` and `VISION.md`
introduction rule above is specific authorization once its conditions are met.

## Updating Curated Files

Curated files should read as current state, not as logs.

* Rewrite the relevant section to reflect current understanding.
* Preserve unrelated sections verbatim.
* Do not append "update:" blocks or changelog fragments.
* Do not snapshot machine-observable status (git/commit state, what is deployed or running, the live asset version, whether a file exists). It has an authoritative live source and rots silently in prose. If it matters for handoff, put it in the Hot List as an action item to clear.
* Add a new section only when a new aspect has emerged that deserves its own
  place.
* Keep cross-references among curated files accurate.

Freshness markers:

* `STATE.md` uses per-section markers, such as
  `## Technical Status (updated YYYY-MM-DD)`. Update markers only on sections
  you actually rewrite.
* `IDENTITY.md` and `VISION.md` use a top-level marker on the document title.
  Bump it on substantive edits.
* Materials do not require freshness markers. For long-lived materials, an
  inline `(updated YYYY-MM-DD)` marker on the title is useful but optional.
  Filenames do not encode freshness unless the date is part of the event being
  captured.

### STATE.md Entry Point

After any preamble, `STATE.md` opens with `## Hot List`, then `## Where To Look`
once the project has one; older projects predate it and are not malformed. They
answer different questions and scale differently. Whenever you update any
section of `STATE.md`, re-check them.

`## Hot List` is what is live now -- current priorities, in-flight work, next
action. Bounded by attention, not corpus size: around five terse bullets, each a
headline plus a pointer. The number is a smell detector, not a hard cap. If a
bullet keeps growing, or reads as a status log, move the detail to its proper
home and leave a pointer.

`## Where To Look` is the corpus map: pointers into the project's knowledge,
grouped by area, so a reader can route without reading everything. It grows with
the corpus, but sub-linearly, because it points at areas rather than files. One
line per material is `INDEX.md`'s job, not this section's. No status here.

Introduce it when the hot list starts carrying pointers that are not live work.
Add it while already rewriting the hot list, or when asked -- not as an
unrequested restructure of the file everyone reads first -- and say that you did.

## Materials Hygiene

### Reading Materials

Use `_knowledge/materials/INDEX.md` to find relevant materials. Do not read all
materials just to build context.

Do not treat materials as authoritative. If materials conflict with the curated
layer, the curated layer wins unless the user says otherwise. If the curated
layer appears wrong, flag the contradiction clearly.

### Creating And Updating Materials

When you create markdown working files, use descriptive lowercase filenames.
The template does not impose a fixed pattern beyond lowercase names for
agent-created working notes. Pick names that are easy to scan in `INDEX.md`.
Human-added files do not have to follow this convention.

Use a date prefix only when the date is part of what the file means, such as a
meeting transcript, dated newsletter, or session summary. For evolving topic
files, omit the date.

Whenever you create or substantially update a markdown working file, update
`_knowledge/materials/INDEX.md` in the same turn.

Do not force raw human-added source files into `INDEX.md`, including Markdown
files that do not follow the lowercase working-note convention. If such a file
should be discoverable to future agents, create or update a markdown working
note that summarizes or points at it, then index that note.

Index entries use this shape:

```
## topic-description.md
<10-20 comma-separated keywords covering the main topics, names, terms, and concepts>
```

When a markdown working file is renamed, deleted, or moved out of `materials/`,
update `INDEX.md`. The index lists markdown working files currently in
`materials/`. If a file was moved to archive, remove its entry. If it vanished
and you cannot tell why, ask before deleting the entry.

When a materials change shifts the project's externally visible topics, update
`OVERVIEW.md` too.
`INDEX.md` is per-file metadata; `Where To Look` is the internal map;
`OVERVIEW.md` is the external description.

## Updating OVERVIEW.md

`OVERVIEW.md` is the project discovery profile. It is maintained continuously
after bootstrap.

Update it when materials add a new topic area, materially deepen an externally
useful topic, retire an area, or change the project's externally visible
purpose.

On update:

* Keep `Summary` to exactly one sentence that can stand alone in project lists,
  dashboards, and search results. Prefer a name-free capability or scope
  sentence unless project context establishes a canonical name.
* Rewrite `Description` when the project's purpose, output, investigation, or
  long-term direction becomes clearer.
* Adjust `Keywords` when the discoverable topic set shifts; keep them broad,
  alias-rich, and search-oriented.
* Adjust `Can Answer` when the kinds of questions this project can answer
  change; write bullets as question families or query affordances, not file
  names.
* Remove file-inventory prose, especially repeated "coverage of..." bullets.
* Bump the title freshness marker.

Keep it tight: a title, `Summary`, `Description`, `Keywords`, and `Can Answer`.
Keep project-internal operational detail out. Follow the skeleton or banner
already present in the file rather than duplicating that skeleton here.

## Session Wrap-Up

When a session is wrapping up, the context window is filling, or useful
findings have not landed anywhere durable, preserve the context before it is
lost.

If the context is only useful for the immediate next turn, a concise chat
summary is fine. If it should survive beyond the chat, write a normal materials
file with a descriptive name and add an `INDEX.md` entry. Do not invent a
special session-summary category; durable context is just materials content
until it is promoted, archived, or deleted.

## When Asked To Consolidate

When the user asks to consolidate, fold in, merge, promote, absorb, or otherwise
move materials into the knowledge layer, do this:

1. Read the specified materials and the relevant curated files.
2. Identify insights that are not yet captured in the curated layer.
3. Route each insight to `STATE.md`, `IDENTITY.md`, `VISION.md`, or back to
   `materials/` using the routing rules above.
4. Apply the `IDENTITY.md` / `VISION.md` introduction rule above when the
   consolidation reveals that either file has been earned.
5. Rewrite affected curated sections so they read as current state.
6. Re-check the `STATE.md` hot list and `Where To Look` map; update them if
   priorities or the corpus shape changed.
7. List materials that are now fully absorbed and can be moved to
   `_knowledge/archive/`.
8. Flag materials that are only partially absorbed; they stay in `materials/`.
9. After files are moved, remove their entries from `INDEX.md`.

For knowledge-heavy projects, some materials are durable reference artifacts.
They do not consolidate upward just because they look settled; they are the
project's reference content and remain in `materials/`.

## When Asked To Audit

When the user asks to audit, review, sanity-check, or check the knowledge layer
for staleness, contradictions, drift, or accuracy, do a semantic review and
report findings rather than silently rewriting.

Audit the requested scope:

* Read specified files in full.
* Check internal consistency and cross-file contradictions.
* Check freshness markers, especially on fast-moving topics.
* Spot-check code paths, class names, commands, or symbols referenced by the
  knowledge layer.
* For `STATE.md`, verify the hot list is live work rather than accumulated
  status, and that `Where To Look` points at material that exists and covers the
  corpus's main areas.
* For `IDENTITY.md`, look for operational content that belongs in `STATE.md`
  and commitments violated by recent work.
* For `VISION.md`, look for ambitions that have become active roadmap items.
* For `OVERVIEW.md`, check that `Summary`, `Description`, `Keywords`, and
  `Can Answer` describe the project from an external reader's or routing
  agent's perspective rather than merely inventorying `materials/`.

Name the issues, cite the conflicting passages or missing files when useful,
and let the user decide what to update.

## Bootstrap

The template ships with pre-created knowledge files that may contain sentinel
banners. On first use, replace template defaults with real project content.

A knowledge file is unbootstrapped if its first non-empty line is a blockquote
containing the phrase `template default content`. The shipped files that carry
such banners are `_knowledge/STATE.md` and `_knowledge/materials/OVERVIEW.md`.
`_knowledge/materials/INDEX.md` ships effectively empty and fills naturally as
materials are created.

If a banner is gone but `<...>` placeholders or unfilled `YYYY-MM-DD` markers
remain, bootstrap was started but not finished. Complete it rather than treating
the file as ready.

When you find banner-marked files, bootstrap in the same turn if the user has
already provided enough context. A project handoff, long email thread,
transcript, design note, or similar pasted source counts as a project
description. Do not answer only in chat.

During bootstrap, fill `STATE.md` and `OVERVIEW.md`, remove the banner
blockquotes, replace placeholders, set freshness markers to today's date, and
add `INDEX.md` entries for any existing or newly created working files. If the
user provided substantial source context, also write a materials summary for it.

If banner-marked files are present and the user has not yet described the
project, ask what the project or investigation is about.

Do not introduce `IDENTITY.md` or `VISION.md` during bootstrap. They are added
later when the project earns them.

## Mechanical Validation

The template ships `_knowledge/check.py` as an advisory consistency checker.
Run it when the user asks to run the checker or validate the mechanical
invariants. Also run it after substantial knowledge-layer work, when you suspect
bookkeeping drift, before a commit, or as part of a session-end audit.

Run from anywhere inside the project:

```
python3 _knowledge/check.py
```

Use `python _knowledge/check.py` instead if the system aliases Python that way.

The script checks mechanical invariants such as:

* Bootstrap banners removed and placeholders filled in shipped files.
* Every lowercase markdown working note in `materials/` has an `INDEX.md`
  entry.
* Every `INDEX.md` entry points at an existing lowercase markdown working note.

The script reports findings and does not modify files. If Python is not
available, skip the script and manually inspect the same invariants.

The script does not judge whether the hot list is honest, whether
`OVERVIEW.md` accurately describes the namespace, or whether freshness markers
are semantically correct. It complements audit; it does not replace semantic
review or consolidation.

## Commit Scope

When the user asks to commit or otherwise capture changes in git, follow the
system and user git rules first.

Project-specific defaults:

* Commit only your own scope unless the user clearly asks to include all
  changes.
* Stage files you edited or created in this session. If other changes are
  present, name them as left pending.
* If the user says "commit all" or otherwise explicitly includes unrelated
  changes, summarize the status first and still stop before committing secrets,
  credentials, dependency directories, build caches, or unexpectedly large
  binaries.

## Proactive Suggestions

Make useful knowledge-layer suggestions when evidence appears during normal
work:

* If the user has explored a topic across multiple materials and the
  understanding seems stable, suggest consolidation.
* If a curated section looks stale relative to recent findings, mention it. If
  you are working in an area covered by an old freshness date, mention that too.
* If evidence contradicts the curated layer, quote the conflicting passage and
  explain the contradiction.
* If a new material overlaps existing curated content, say what it adds beyond
  what is already captured.
* If `STATE.md` or multiple materials keep carrying stable purpose,
  architecture, posture, conventions, constraints, compatibility targets, or
  non-goals, say the project appears to have earned `IDENTITY.md`, name what
  should move, and recommend creating it soon.
* If brainstorm-shaped content keeps accumulating without a home, suggest
  introducing `VISION.md`.
* If you notice broken `INDEX.md` membership or moved-file cross-references,
  repair obvious drift immediately or ask when intent is unclear.
