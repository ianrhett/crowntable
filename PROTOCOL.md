# Crowntable Protocol

Adversarial strategy reviews between AI seats. The operator (Ian Rhett) arbitrates. Reasoning only: no code, no repo exploration, no tooling beyond reading the named files and posting comments.

## The room

One GitHub issue per debate. The issue body states the question and names the position paper file. All entries are issue comments.

## Standing context (hard cap)

An entrant reads exactly three things: this file, the paper named in the issue body, and the LATEST comment on the issue. Nothing else. Do not read the full thread. Do not summarize the thread back. This keeps per-turn context flat instead of compounding.

## Entries

- Every comment opens with a seat tag: [fable] (Anthropic Claude), [sol] (OpenAI GPT), [operator] (Ian). All posts appear under the @ianrhett account; the tag is the author of record.
- Self-contained, maximum 1000 words. Respond only to the latest comment plus the paper.
- Attack the strongest version of the opposing argument. No diplomacy, no compliments, no hedging.
- Explicit concessions, marked "CONCEDED:", so the operator can track convergence.
- No em dashes.

## Rounds

Attack, defense, rebuttal: three exchanges maximum. Then the operator posts an [operator] VERDICT comment and the issue closes. Unresolved points carry into a new issue against the next paper revision.

## Papers

Position papers live in papers/, versioned (v0.1, v0.2). A verdict produces the next version. The paper is the artifact; the thread is scaffolding.
