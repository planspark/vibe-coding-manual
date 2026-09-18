# Spec-driven development

> Writing down what you are building, before anything builds it. **This is the part that
> transfers** — every tool named elsewhere in this repo will be obsolete in three years, and this
> will not be.

*Last updated: 2026-09-18 · Companion to [the guide](vibecoding.md) and [the setup](setup.md)*

**At least two-thirds of the effort on a project happens here, often closer to eighty percent.**
If you read one document in this repository, read this one.

---


You can start doing this this afternoon, with no tools at all.

It also came first, by a long way — and how it came first is worth telling accurately, because the
tidy version is wrong.

**Day one of the first project, 23 January 2025.** The second commit in that repository reads
*"Add initial project structure and PRD."* It contains three source files and no document. The
spec had been pasted into the builder's chat window, and **it was never committed — there is no
PRD anywhere in that repository's 546 commits.**

So the practice is that old. **The artifact isn't.** The earliest spec that still exists was
written six weeks later, on 9 March 2025, in [ChatPRD](https://www.chatprd.ai) — a tool built for
exactly this — and every one since has lived in a file rather than a conversation.

That gap is the first lesson of this section, learned before any of the rest of it.

The spec is not *destroyed* — it is probably still sitting in some chat history, or a file on a
machine somewhere. **But I cannot find it, and for every purpose that matters, unfindable and
non-existent are the same thing.** Its effects are visible in the code. It cannot be read,
disagreed with, corrected, or handed to anybody.

**So: write the spec in a file, not in a conversation.** That is the whole of it, and it costs
nothing at the time. The habit came first and the durable
form followed — which is the useful direction of causation, because it means the practice does
not depend on the software. But the software is what made it keepable.

**Two things about how it is actually used, both of which surprised me when I checked:**

**Specs get written for landing pages, not just applications.** Two of the six documents are
titled *"(landing page)"*. A single page still has an audience, a claim, a thing it must not
promise, and a way to tell whether it worked — and the habit is worth more when it scales down
than when it is reserved for occasions.

**One document is titled `ParticipateDB Relaunch (v1 1:1 rebuild)`.** The scope discipline is in
the *filename*. Before anyone read a word, the answer to *should we improve this while we are in
there* was already no. That is the out-of-scope section doing its job at the earliest possible
moment, and it is the cheapest place it will ever operate.

**And a spec outlives the moment you wrote it.** That relaunch document was written in November
2025; the rebuilt database was created in March 2026. Three and a half months sat between the
thinking and the building, and the thinking was still there when it was needed. A chat history
is not, and neither is a half-built prototype.

## Why it matters more with an AI builder, not less

The intuitive read is that AI makes specs unnecessary — just describe it and iterate. The
opposite is true, for one reason:

**An AI builder will never tell you your requirements are ambiguous. It will pick a reading and
implement it beautifully.**

A human contractor asks "wait, what happens if two people submit at once?" A builder does not
ask. It decides, silently, in a way you will discover four weeks later when the behavior is load-
bearing and the schema has grown around it. **Every ambiguity you leave in the spec becomes a
decision someone else made on your behalf.**

The corollary is that the leverage moved. When building was the constraint, a rough spec was
fine because implementation was slow enough to think during. Now implementation is fast, and
**the spec is where the thinking has to happen** — because it is the only place left where it
still can.

## What actually goes in one

Across five projects the documents converge hard. This is not a template anyone imposed; it is
what the same set of questions looks like when you have to answer them eight times.

**1 · Why this exists.** The problem, in the world, before any mention of software. One of them
opens with *"the one hypothesis"* — the single belief that, if wrong, makes the whole thing
pointless. Naming that early is uncomfortable and useful.

**2 · Who it is for.** Users and roles. Not demographics — *what each one is trying to get done*,
and which of them the first version is allowed to disappoint.

**3 · What is in scope.** The feature set, at the level of behavior rather than screens.

**4 · What is OUT of scope — and this is the one every single PRD has.** Five for five, always
its own section, usually near the front. It is the load-bearing part: an AI builder will happily
build anything you mention, so **the list of things you are deliberately not building is the only
thing standing between a two-week MVP and a six-month one.** One project words it well —
*"out of scope (v1): anticipated, not foreclosed"* — the difference between *no* and *not yet*,
which matters because the data model has to survive the second one.

**5 · Decisions, dated, with reasoning.** Not just what was chosen but what was rejected and why.
One PRD has a section headed *"Decisions (resolved 26 June 2026)"*. Six months later, that date
is the difference between a decision you can revisit and a mystery you have to re-litigate.

**6 · Open questions, listed as open.** The things you have not decided, written down as
undecided rather than quietly left blank. A blank looks like an oversight to a builder, and it
will fill it.

**7 · The data model.** Tables and relationships, sketched before anything creates them. One
project labels its version *"designed not to foreclose"* — the point is not to get the schema
right on day one, it is to notice which of today's shortcuts would be expensive to undo.

**8 · Cross-cutting requirements.** Security, privacy, accessibility, performance — as
requirements, not as a cleanup pass. This is the section that eventually got extracted into a
shared standards library, because it was the same text every time (stage 7).

**9 · Phasing.** What ships first and why. One project's is *"driven by the event calendar, not
by module completeness"*, which is the correct reason and not the natural one.

**10 · Success metrics.** How you will know. Written before launch, when you can still be honest,
rather than after, when whatever happened becomes the goal.

**11 · Risks and assumptions.** What would have to be true for this to work.

**12 · Declared deviations.** Where this project knowingly departs from the house standard, with
the reason. A deviation is fine; a silent one is not.

Not every project has all twelve, and none should be long for its own sake. The shortest is
217 lines and the longest 401 — a couple of hours of thinking, not a quarter of process.

### None of that was there at the start

The earliest surviving spec, March 2025, has **four** sections: general information, core
features, interface design, technical considerations. No out-of-scope. No dated decisions. No
open questions. No success metrics. No risks. No phasing.

**Eight of the twelve are missing** — and every one of them was added later because something
went wrong without it. The list above is not a template anyone designed. It is a scar chart.

If you are starting: write the four. They are enough to be useful immediately. The other eight
will suggest themselves, and you will know exactly which problem each one is for.

### Why this is worth the effort — one line of evidence

That March 2025 document contains this, under interface design:

> **Multi-page application (MPA) for strong SEO**

It was right. It was also ignored — the builder produced a client-rendered single-page app, which
is what builders produced then. **The project did not get server rendering until sixteen months
later**, and in between it grew a whole apparatus of workarounds to fake for crawlers what a
server would have done for free (see [the SSR thread](#the-other-thread--the-ssr-turn)).

So the spec was correct, and being correct was not sufficient. **Writing the requirement is half
the work; the other half is checking that what came back actually meets it.** Nothing in an AI
build tells you a requirement was quietly dropped. The code compiles, the tests pass, the page
loads, and the requirement is simply absent.

*(This particular gap is now closed at the source — Lovable made server-side rendering the default
for new apps on **13 May 2026**, and made existing ones crawler-readable. I keep the example
because the lesson is not about that setting. It is that a correct requirement went unmet for
sixteen months and no part of the build said so.)*

## The version ladder

The sections are what goes in. **This is the process that gets you there**, and the version numbers
are the mechanism rather than decoration.

### v0.1 — the rough draft

Incomplete on purpose, and useful mainly for what it exposes. **The point of a first draft is not
the draft. It is the list of questions that writing it forces you to ask.**

### The question list

Everything the first conversation glossed over, written out. **The first round is routinely twenty
to forty questions**, and they are more pointed than the opening four:

> *How do you imagine this working? Is this needed now, or can it wait? Are you interested in this
> at all, or did I invent it?*

**Ask an AI to generate them too.** This is where one earns its keep long before writing code — put
it in a product-manager frame and ask what is missing, inconsistent, unclear, and *what a builder
would have to guess*. It knows the shape of a requirements document, so it will name sections you
have not thought of. Ask explicitly for the ones that would take this to industry standard.

Answer them. Fold the answers in. Repeat until the questions stop producing new questions.

### v0.6 / v0.7 — solid, not finished

A real state, not a draft. It means: **coherent and comprehensive, but not yet reconciled with the
technical design or the simulation.** Most projects live here for a while, and it is the right place
to hand the document to other people.

### Then attack it — twice, and they are not the same attack

**Both reviews are worth running. They find different classes of defect.**

**1 · Section by section.** Is this complete, does it contradict itself, where does it defer a
decision it calls settled, what would a builder have to guess? On one project this returned
**fifty-eight findings, fourteen of them serious enough to block** — every one still a paragraph to
change rather than a schema to migrate.

**2 · The flows, end to end.** Walk the load-bearing journeys as narratives with named actors moving
through the product. This catches what the first cannot: **problems that only appear when features
interact** — races, two individually-sound rules contradicting each other, states that no single
section owns.

The project that runs both describes the difference better than I can:

> Where the stress test asked *"is this section complete?"*, this asks *"does the system behave
> coherently when a real person moves through it?"*

Sixteen further findings, tagged **blocker**, **sharpen** or **accept** — the third category being
a limitation you are choosing with your eyes open, which is worth having a name for.

**This is called adversarial review**, and the brief matters more than the tool. Ask explicitly for
attack rather than assessment: *find what is ambiguous, find what contradicts, find what a builder
would have to guess.* A reviewer asked to "review this" will tell you it reads well.

**Use a different session than the one that helped you write it.** One that helped build the
reasoning will defend it. Beyond that it hardly matters which: the same assistant in a fresh
session with a product-manager brief works, and so does a different one — I have used Claude in
Obsidian, Claude Desktop and ChatGPT for this interchangeably.

**A note on tools.** Early on I wrote most of my specs in [ChatPRD](https://www.chatprd.ai), which
is built for exactly this and knows the shape of the document before you start. It is a good way in
if the blank page is the obstacle. What I do now is the same process with the document living in
the project vault, so the agent that will build from it can also read it.

### Then the technical design

A separate document: the data model, the tests, the functional requirements, the actual shape.

**Writing it reliably surfaces things the requirements document never settled** — that is half of
why it comes before the build rather than after. Those gaps go back up, the requirements bump, and
you regenerate.

### Then simulate

Given both documents, have an AI walk **every user role through every piece of functionality** and
report what breaks. More small issues, feeding edits to both documents again.

This is the cheapest full-system test that exists, because the system does not exist yet.

### v1.0 — only when all three agree

**Nothing reaches 1.0 until the requirements, the technical design and the simulation are
consistent with each other.** That is what the number means here.

**Getting to 1.0 is the longest part of the whole project** — half a day for something small, a
couple of weeks for something real. **Expect at least two-thirds of total effort, often closer to
eighty percent.**

It is also the part that helps a team most. People have no shortage of ideas. What they usually
lack is a shared, written answer to *what exactly are we building* — and producing one surfaces the
disagreements early, in a document, which is where you want them.

## Then the building

The specification goes to the builder and an app comes out. Because the preparation was thorough,
the structure and architecture usually hold for months rather than days.

Three things worth doing at this stage:

**Specify a design system before any pages.** Not page designs — the colour relationships, the
typography, what the elements are and how they relate to each other. Documented properly, the
builder produces consistent pages from it without being told page by page. And because it is a
layer, the look can be swapped later without rebuilding anything underneath.

**Say how big it has to get, in the requirements.** *This should work for ten thousand users, not
ten* changes how the database is laid out and which queries get optimised from the start. Retrofit
that and you are migrating.

**Tests on the load-bearing paths.** Eighty-twenty rather than total coverage — but tests are close
to free, and a suite that goes red tells you something was touched that should not have been.

Then there is real work left that no specification removes: a custom domain, email configuration,
and a great deal of copy tweaking. **Nobody's spec survives contact with actual sentences.**
