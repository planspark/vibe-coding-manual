# The vault-per-project setup

> One folder per project. An AI agent that lives inside it, remembers what you decided,
> and knows what it may do without asking.

*Last updated: 2026-09-18 · Tested with: Obsidian 1.13.7 (installer 1.11.7), Claude Code (Max), macOS*

This is the shared setup behind everything else here. It is not about building web apps —
it works the same for a language-learning vault, a research project, or a book. The
[vibecoding guide](vibecoding.md) starts from this and adds the app-specific stages.

**What you get:** an assistant that opens a session already knowing your project's history,
decisions and constraints — instead of one you re-brief every time.

---

## 1. Why a vault, and not just a chat window

A chat window forgets. Worse, it only ever sees what you paste into it, so it advises you
about a version of your project that lives in your head.

A vault inverts that. The agent reads the actual files: your specs, your decisions, your
notes, and — if you are building software — the code itself. **It stops being a smart
stranger and starts being someone who works here.**

Three things make that real, and all three are just files:

| | |
|---|---|
| **A charter** | what this project is, and how you work |
| **Memory** | what it should still know next month |
| **Session notes** | what happened, while it is happening |

Everything below is a text file in a folder. There is no database, no app, no lock-in. You
can read all of it in any editor, and so can the next tool you use.

## 1a. Where this came from, and the fastest way in

**I did not invent this.** The shape — a folder of plain markdown, an AI agent living inside it,
memory and session notes as files — comes from [Peter Kaminski](https://peterkaminski.ai), whose
excellent March/April 2026 course
**[Agentic AI with Pete](https://learn.peterkaminski.ai/)** got me started on the Obsidian/Claude
setup. He has been building wiki-shaped knowledge systems for a long time, well before the idea
got a name and a famous essay.

**If you are starting from nothing, start with his [PKAI Starter Kit](https://peterkaminski.ai/starter-kit/).**
You install it, then talk to the assistant inside your own vault and it walks you through setting
itself up — which is a much gentler on-ramp than a document like this one.

**Then point it here.** The starter kit is aimed at a personal assistant: your notes, your life,
your questions. Everything in this repository is the same machinery aimed at *building software* —
one vault per project, a code repo inside it, an agent that reads your specs as well as your code.
Once the kit is running, you can tell your assistant that is the direction you want to go.

## 2. Prerequisites

- **[Obsidian](https://obsidian.md)** — free. It is a markdown editor over a folder; the
  folder is the point, and Obsidian is just a pleasant way to see it.
- **[Claude Code](https://claude.com/claude-code)** — the CLI. In Obsidian, the community
  plugin puts it in a sidebar, which is where most of this happens.
- **git** — comes with macOS developer tools.
- A **Claude subscription**. This is the real cost; see §9.

## 3. The shape

```
your-project/          ← the vault. Its own git repo.
├── CLAUDE.md          ← the charter. Read at the start of every session.
├── memory/            ← what persists
│   ├── MEMORY.md      ← the index, always read first
│   └── <slug>.md      ← one file per fact
├── sessions/          ← one note per working session
│   └── YYYY-MM-DD-NNN.md
├── docs/              ← specs, plans, research
└── app/               ← the code repo, if there is one. A separate git repo.
```

**One vault per project.** Not one vault for everything. The whole benefit is that the agent
sees *this* project entirely and nothing else — which is also the safety boundary (§6).

**The code nested at `app/`.** Your builder — Lovable, or whatever — owns that folder and
syncs it to GitHub. The vault holds everything the builder never sees: why you decided
things, what you rejected, what is planned. That gap is most of the value.

## 4. `CLAUDE.md` — the charter

This is the single most important file. It is read at the start of every session, so
**everything in it costs you on every session** — which is the discipline that keeps it short.

A working order, roughly stable to most-volatile:

1. **What this project is** — two or three lines, plain
2. **Read first** — the two or three documents that matter, and when
3. **Hard constraints** — the things that must never happen
4. **Stack and layout** — where things live
5. **Commands** — how to run, test, build
6. **How we work** — tone, register, what you want more or less of
7. **Authority boundaries** — §6
8. **Current status** — last, because it rots fastest

**Point, do not restate.** If the strategy lives in `docs/strategy.md`, the charter says so
in one line. A charter that duplicates another document is a file you now have to update
twice, and you will update it once.

**Facts about the project go stale silently.** A "current phase" section saying *no code has
been written yet* is actively harmful three weeks later — the agent will believe it. Put
volatile facts last, and expect to prune.

## 5. Memory — what persists

`memory/MEMORY.md` is an index, one line per memory. Individual memories are their own files.

```markdown
---
name: us-english
type: feedback
description: US English in everything, no exceptions
---

Use US English throughout — theater, organize, program, catalog.

**Why:** the audience is primarily US-based and mixed spelling reads as careless.
```

Types worth having: **user** (who you are), **feedback** (how you want it to work),
**project** (decisions and their reasons), **fact** (discrete things to remember).

**What makes this work is the index.** The agent reads `MEMORY.md` every session — a few
hundred words — and opens individual memories only when relevant. Reading everything every
time does not scale and is the reason "just put it all in the prompt" stops working.

**Save during the conversation, not at the end.** The end may not come.

**Delete memories that turn out wrong.** A confidently wrong memory is worse than no memory,
because it gets acted on without being re-checked. Mine have been wrong more than once.

## 6. Authority boundaries — the part most people skip

Write down what the agent may do without asking. A table in the charter:

| Action | Authority |
|--------|-----------|
| Read and edit files **in this vault** | Autonomous |
| Read anything outside this vault | **Ask first** |
| Create or edit files outside this vault | **Always ask** |
| Run shell commands | Autonomous if reversible, ask if destructive |
| Git commits in this vault | Autonomous |
| `git push` | **Ask first** |
| Production database writes, deploys | **Ask first** |
| Send email or messages on your behalf | **Always ask** |
| Spend money, delete data | **Always ask** |

Two reasons this matters more than it looks:

**It makes speed safe.** Without a table, every action is either a prompt you click through
without reading, or a hesitation. With one, the reversible things happen at full speed and
the irreversible ones stop.

**It is where trust gets recorded.** Start narrow. When something proves itself, widen it
deliberately and write down that you did. Authority should grow because it was earned, not
because you got tired of clicking.

## 7. Session notes — a running draft

One note per session, `sessions/YYYY-MM-DD-NNN.md`, **opened at the start and appended to as
you go.** Not written at the end.

Under the heading while you work:

```markdown
**Status:** draft — in progress
```

Delete that line when you close the note. Its absence means final — and **a note still marked
draft means that session ended abruptly**, which is exactly when you most want a record. Have
the agent check for one at the start of every session and offer to finish it from `git log`.

A shape that holds up:

```markdown
## Goal          — what we set out to do
## What we did   — grouped by thread, not chronology
## Decisions     — and the reasoning. The part git cannot reconstruct
## Open / next   — what is unfinished, and what the next session starts on
```

**That last line earns its keep more than the rest combined.** A session that opens with
"so, where were we" spends its first ten minutes rebuilding state the previous session had in
hand. Ending with *what happens next* costs one sentence, once.

**Favor why over what.** The diff already records what changed.

## 8. Version control — both repos

```bash
cd your-project && git init -b main     # the vault
```

The vault is usually the *less* backed-up of the two and the more painful to lose: `app/`
syncs to GitHub through your builder, while the vault exists only on your laptop until you do
this. Specs, decisions, research, and months of accumulated context.

`.gitignore` in the vault:

```gitignore
/app
.env
**/.env
.obsidian/plugins/
.DS_Store
```

**`/app` with no trailing slash.** A nested git repo is a *gitlink*, and `/app/` does not
match one. Get this wrong and git quietly stores a pointer to a commit instead of ignoring
the folder.

Then decide about a remote **deliberately**. Local-only gives you history and undo but no
backup. That is a legitimate choice; drifting into it is not.

## 9. What it costs

- **Obsidian** — free.
- **Claude subscription** — the real line item. Claude Code is included in the paid plans.
- **Your builder** — Lovable and similar charge per message. This setup exists partly to
  spend fewer of them; see the [guide](vibecoding.md), stage 2.

The honest version: this is not free, and the subscription is the floor. What it buys is
that the expensive tool stops being where you do the thinking.

## 10. Your first session

- [ ] `git init -b main` in the vault, `.gitignore` with `/app` bare
- [ ] `CLAUDE.md` — what this is, read-first, how we work, authority table
- [ ] `memory/MEMORY.md`, even if empty
- [ ] `sessions/` with today's note, opened before you start
- [ ] Ask the agent to read the vault and tell you what it thinks the project is —
      **its answer is a test of your charter, not of the agent**
- [ ] Commit

## 11. What goes wrong

**The charter grows until nobody reads it.** Length is a symptom. If it is long, something in
it belongs in a document the charter points at.

**Status sections lie.** They were true when written. Date them or delete them.

**The builder cannot read your vault.** If you are using Lovable or similar, it only sees
`app/`. Anything it must obey needs to be in an instructions file *inside* `app/` — a charter
in the vault governs your sessions and none of its.

**Memory drifts from reality.** Before acting on a memory that names a file, a person, or a
number, check that it is still true.

**A check that could not run is not a check that passed.** This one generalizes past
software: when the agent tells you something is fine, it is worth knowing whether it looked,
or whether it could not look and said nothing.

---

## Where next

- **[The vibecoding guide](vibecoding.md)** — the eight stages, if you are building web apps
- **Virtual Spanish tutor** — the same setup pointed at language learning *(coming)*

*Questions, corrections and forks welcome. This is a living document — the date at the top is
the one that matters.*
