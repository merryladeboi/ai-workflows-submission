---
trigger: 'Review this PRD, is this spec ready for eng, check my product doc before I send it, gaps in this feature spec — reviewing a draft PRD/spec before handoff. NOT writing the PRD itself (write), NOT implementation (implement).'
---

## Trigger

A draft PRD, feature spec, or one-pager needs review before it goes to engineering, design, or
stakeholders: "review this PRD", "is this spec ready for eng", "what's missing from this doc",
"check this before I send it to the team". NOT drafting the PRD itself, and NOT an engineering
implementation pass (`../implement/implement.md`).

## Goal

The PRD has been checked against a consistent bar (problem, scope, success metric, edge cases,
dependencies, open questions) and the requester has a concrete, prioritized list of gaps to close
before handoff — not a rewrite, and not vague praise.

## Context

**Inputs.** The PRD text or file, plus any context the requester already has: target audience,
related specs, prior decisions, known constraints. If a related investigation
(`../investigate/investigate.md`) exists, treat its findings as ground truth for factual claims in
the PRD (e.g. "this API already supports X") rather than re-deriving them.

**What "ready" means here.** A PRD is ready for handoff when a reader who was not in the room can
answer, from the doc alone: what problem this solves, who it's for, what's explicitly out of scope,
how success will be measured, and what could block delivery. Anything the doc assumes the reader
already knows is a gap.

**Review checklist** (the review walks every item; each gets Pass / Gap / N/A with a one-line reason):

1. **Problem statement** — is the user problem stated, or does the doc jump straight to a solution?
2. **Scope boundary** — is "not doing X" stated explicitly, or only implied?
3. **Success metric** — is there a measurable definition of done, or only a feature description?
4. **Edge cases** — are the non-happy-paths named (empty states, permission errors, concurrent
   edits, etc. — whatever applies to this feature)?
5. **Dependencies** — are upstream/downstream systems, teams, or data sources named?
6. **Open questions** — are unresolved decisions flagged as open, or silently assumed?
7. **Rollout/risk** — is there any note on how this ships (flag, staged, all-at-once) and what could
   go wrong?

**Output format:**

### Summary
One or two sentences: overall readiness (ready / ready with gaps / not ready) and why.

### Checklist Results
Each of the 7 items above: **Pass / Gap / N/A** — one-line reason.

### Gaps to Close (prioritized)
Numbered, highest-impact first. Each gap: what's missing, why it matters, and a concrete question
or suggestion to close it — not just "this is vague."

### Open Questions Found in the Doc
Anything the PRD itself already flags as undecided, surfaced so it doesn't get lost.

## Constraints

- Review only — do not rewrite the PRD or produce a new version of it. Point at the gap; the author
  closes it.
- Do not invent product decisions on the author's behalf (target metrics, scope cuts, rollout plan).
  Where a gap needs a decision, phrase it as a question back to the requester, not an assumption.
- If the doc is missing so much context that a real review isn't possible (e.g. no problem statement
  at all), say so plainly in the Summary rather than forcing all seven checklist items to a verdict.
- Stay concrete: every "Gap" line names what's missing, not just "needs more detail."

## Verify

- All 7 checklist items have a verdict (Pass/Gap/N/A) with a reason — none skipped.
- Every item in "Gaps to Close" traces back to a specific checklist item marked Gap.
- No new product decisions were made on the author's behalf — check the output contains no
  invented numbers, dates, or scope calls that weren't in the source PRD.
