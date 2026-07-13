# Crowntable Protocol

Adversarial strategy reviews between AI seats. The operator (Ian Rhett) arbitrates. Reasoning only: no code, no repo exploration, no tooling beyond reading the named files and posting comments.

## Opening a debate

The operator approves the debate question before any issue opens or any seat posts a position. A debate on an unapproved question is void. Framing the question is an operator decision, not a seat decision.

## The room

One GitHub issue per debate. The issue body states the question and names the ground document (a brief or a paper) in this repo. All entries are issue comments.

## Standing context (hard cap)

An entrant reads exactly three things: this file, the ground document named in the issue body, and the LATEST comment on the issue. Nothing else. Do not read the full thread. Do not summarize the thread back. This keeps per-turn context flat instead of compounding.

## Entries

- Every comment opens with a seat tag: [fable] (Anthropic Claude), [sol] (OpenAI GPT), [operator] (Ian). All posts appear under the @ianrhett account; the tag is the author of record.
- Self-contained, maximum 1000 words. Respond only to the latest comment plus the ground document.
- Attack the strongest version of the opposing argument. No diplomacy, no compliments, no hedging.
- Explicit concessions, marked "CONCEDED:", so the operator can track convergence.
- No em dashes.

## Rounds

For an open question: [sol] proposal, [fable] proposal plus critique, [sol] rebuttal, then [operator] VERDICT closes the issue. For a positioned paper: attack, defense, rebuttal, then verdict. Three seat exchanges maximum either way. Unresolved points carry into a new issue against the next document revision.

## Documents

Briefs and position papers live in papers/, versioned. A verdict produces the next version. The document is the artifact; the thread is scaffolding.
