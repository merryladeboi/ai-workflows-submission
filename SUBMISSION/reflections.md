# Reflection: AI-Workflows Repository

## How I approached this

I cloned the repo and read through the workflow files with Claude (Anthropic's AI) as a thinking
partner — I fed it the workflow markdown files, asked it to explain what system was actually being
described, then used it to stress-test my own read of the repo (e.g. checking whether every file
actually conformed to the standard the repo itself defines in `work-with-workflow.md`). That
back-and-forth is also how I found the frontmatter inconsistency noted below — I asked Claude to
verify my assumption about which fields discovery reads, rather than taking it at face value.

## What this repository is

It's not a single workflow — it's an **operating system for how AI agents get delegated work**.
`work-with-workflow.md` is the meta-spec: every task type (bug fix, PR, planning, investigation...)
is captured as a markdown file with exactly five sections (Trigger → Goal → Context → Constraints →
Verify), and an agent auto-discovers the right one by matching a request against each file's
`trigger` frontmatter. The philosophy underneath it: script anything that can be enumerated as a
rule, and only fall back to an LLM step when judgment is genuinely required — and every such step
must justify itself inline (`LLM needed because...`). It's governance for AI-driven work, not a
single-purpose tool.

## Use cases for my professional life (Product Manager, no coding background)

- **`planning.md`** — turns a vague ask ("plan the Q3 rollout") into a dependency-mapped task list
  (Parallel / Blocked by / Blocks per task). I'd use this directly for feature launch planning and
  roadmap breakdowns — it's language-agnostic to my not being an engineer.
- **`investigate.md`** — read-only recon with a strict output contract (exact file/line references,
  no paraphrasing, written for someone who's never seen the material). This maps almost exactly to
  how I'd want AI to summarize a stack of user interviews, support tickets, or a competitor's docs
  before I write a PRD.
- **`implement.md` / `bug-fix.md`** — this is the part that's genuinely new to me as a non-engineer:
  the guardrails (ambiguity gate that refuses to touch code if the ask isn't clear; test-red-before-fix
  discipline; a report that lists every file touched) are what make it *safe* for someone like me to
  delegate real code changes to AI and trust the result, instead of needing to read the diff myself.
- **`gmail-workflow.md` / `gsheet-workflow.md`** — the most immediately usable, zero setup needed
  beyond OAuth: inbox triage with a dedupe-before-classify step, draft-only replies (never sends),
  and a Sheets CLI I could use for a lightweight spec/feature tracker without opening a spreadsheet
  UI at all.
- **`specs-optimization.md`** — conceptually the most relevant one to SUN Mobility's transformation
  specifically: it runs AI "probes" against a documentation corpus to measure where AI agents get
  lost navigating it, then recommends re-categorization. That's the exact question a company going
  AI-native should be asking about its own product specs and internal docs.

## Use cases for personal life

- `gmail-workflow.md` for inbox triage/cleanup with the same drafting-not-sending safety net.
- `gsheet-workflow.md` for a personal budget or trip-planning tracker, driven by chat instead of
  manual spreadsheet editing.
- `planning.md` for something like an apartment move or a personal project, where I want a task
  breakdown with dependencies made explicit before I start.

## What I'd change

1. **Frontmatter inconsistency that breaks discovery.** `work-with-workflow.md` states plainly that
   discovery only reads the `trigger` key now — *"`description` is no longer read"*. But
   `gsheet-workflow.md` and `gmail-workflow.md` both still use `description:` in their frontmatter,
   with no `trigger:` field at all. As written, these two workflows are invisible to the discovery
   mechanism the rest of the repo relies on. This is a small fix (rename the field, keep it under
   170 chars) but it's a real contradiction between the repo's own stated rule and its own files.

2. **Verification assumes terminal fluency.** Every `Verify` section is raw bash/CLI commands. That's
   appropriate for the engineering workflows, but if this system is meant to extend to non-engineers
   (which the personal/professional use cases above suggest it should), I'd want a plain-language
   summary layer on top — an agent reporting "3/3 checks passed" in prose — so someone without shell
   experience can still trust that Verify actually ran and passed, not just take it on faith.

3. **`planning.md`'s structure is generic but its vocabulary isn't.** The breakdown pattern —
   one-line goal, numbered tasks with explicit dependencies, a risks section — is genuinely reusable
   for any kind of decision-making, not just engineering work. But the fields are locked to a software
   context: `Files to Modify` / `New Files` don't apply to something like a budgeting decision or
   interview prep, and `Parallel: yes/no` + `Blocked by/Blocks` (clearly built for coordinating
   multiple engineers or agents) is overhead for a plan that's realistically linear. I'd generalize
   this workflow — or split it into two variants, e.g. `planning-engineering.md` (as it is today) and
   a leaner `planning-personal.md` — so the same reasoning structure can be reused for everyday
   decisions without carrying fields that don't apply.

4. **Personal-development use cases don't have an obvious home yet.** Things like mock interview
   practice, assessing job fit, or budgeting guidance don't map cleanly to the workflows in the
   scope I reviewed (`investigate.md` is read-only recon on files/code, `implement.md` edits files,
   `planning.md` is the software-shaped breakdown above) — nor do they fit the request/response
   shape most of these are built around. They're closer to an interactive, back-and-forth coaching
   session than a single delegated task with one final report. I only reviewed a subset of the
   repository, so this gap may already be covered elsewhere (e.g. in workflows built around other
   tools). If it isn't, I'd suggest a distinct category for this pattern — something interactive by
   design, rather than trying to force it into the read-once/report-once shape the current workflows
   share.

5. **`implement.md`'s output is written for a reviewer who can already read a diff.** The report format
   it produces — "Files Changed" as a bare list of paths, "Notes" for anything else — assumes the
   person reading it is technical enough to open those files and understand what changed just from the
   path and a one-line description. That's a reasonable default for handoff-to-review, but it doesn't
   work if the person delegating the task is a non-engineer (a PM, for instance) who needs to trust
   that the right thing happened without opening the code themselves. I'd add one requirement to the
   Goal/Constraints of `implement.md`: every completion report opens with a plain-language summary —
   one or two sentences describing what changed and why, in terms someone without engineering context
   can verify against their original ask — before the technical "Files Changed" list. Something like:
   "Plain-language summary: the signup form now shows an error message if the email field is empty,
   instead of failing silently." This costs almost nothing to add (the AI already knows what it did),
   but it changes who can actually use this workflow to delegate work with confidence — which matters
   if AI-native tooling is meant to extend past the engineering team.
