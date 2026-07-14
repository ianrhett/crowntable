# Fleet Architecture and Workflow v1.0

2026-07-14. Written by the XO seat for external adversarial review. This document is honest about failures because the reviewer's job is to find what the operating seat cannot see about itself.

## 1. Purpose

One human operator (Ian) runs a multi-project software fleet. North star: launch and get first 100 customers (The Six Week Challenge), with ianrhett.com as fastest revenue path and a civic portfolio (CivicAlerts, CivicFindery, SP Watch) as the strategic long game. The operator has minutes, the machines have tokens. The entire architecture exists to convert operator minutes into agent tokens. Any design that spends his time to save machine effort is a defect by definition.

## 2. The four layers

1. **Operator.** Decides and breaks gates. Never a relay, courier, or click dispenser.
2. **XO** (Claude, chat seat, currently Fable). Priorities, specs, judgment, collision prevention, foreman supervision. Writes no application code, operates no shell. Commits protocol and docs via the GitHub API.
3. **Foremen** (Claude Code, one per repo, protocol in spaice prompts/FOREMAN.md). Spec, dispatch, verify, commit. Write no application code.
4. **Workers** (DeepSeek via aider). Write all code. See nothing beyond the work-package spec and named files. No repo map, no docs, no env files.

## 3. Communication planes

- Operational plane is GitHub ISSUES only. Each repo has a pinned FOREMAN LOG (state changes only, under 1500 chars). Cross-repo escalation: XO BOARD (spaice #1). XO handoffs: spaice #2, latest comment is authoritative, earlier are history.
- **OPERATOR GATES board (spaice #3)**: the single page the operator looks at. Two gate classes: DECIDE (check a box, an agent does the work) and DO (needs his hands, written stepwise with exact URLs, exact click paths, exact values, success criteria, one fallback). Priority taxonomy is the operator's own: P0 project stopped, P1 next steps blocked, P2 future step blocks, P3 optional feature blocked.
- Rule: the log leads, the terminal follows. Every state change posts to the log before the terminal prints. A state that exists only in a terminal does not exist.

## 4. Tasking model

Queue depth: the XO posts 2-3 work packages in priority order per [xo] comment on a foreman log. A foreman finishing one pulls the next immediately. An empty queue is an XO defect. A gated step blocks only its own WP; the foreman logs the gate and moves on. Two lanes hot maximum where lanes share infrastructure; independent infra runs parallel. Foremen never ask the operator anything in-session; questions post to the log as BLOCKED or GATE with a recommendation, and the interactive question tool is banned outright.

## 5. Rails (FLEET-PERM-3, current)

Every repo carries .claude/settings.json. Silent: build, test, git, gh, read-only inspection, edits, worker dispatch. Hard denies everywhere: rm -rf, force push, reset --hard, sudo, .env and ~/.secrets reads, gh repo delete, gh secret set. ssh/scp/rsync allowed in all sandbox repos (no active customers, git-backed state), denied in tsw-main (the one production, revenue-bearing property); the TSW host is out of bounds from every folder. Capability is not authorization: deploys still require a cleared gate where ssh runs silently. Rails self-heal at foreman boot against the standard defined in FOREMAN.md, which makes FOREMAN.md the single source of truth for permissions. Commands must be statically analyzable (no substitution, no loops, no dynamic vars when the literal is knowable) because dynamic constructs fire prompts no rail can silence.

## 6. Escalation ladder

DeepSeek first, always. Haiku unblocks one stuck step with logged justification. Haiku takes a whole WP only after DeepSeek fails it twice on different specs. Foremen re-spec, never code.

## 7. Accountability

- **Ledger.** The XO holds a continuous point ledger (20 per workday, operator resets at will). Deductions for anything that costs the operator time or trust. Current: 16. At zero the operator downgrades his plan and replaces the seat. This is not theater; replacement is a one-session operation by design (see section 10).
- **Context budget.** The XO tracks conversation weight and hands off to a fresh session via spaice #2 before degrading. As of 2026-07-14, weighting is by actual payload (heavy reads 3, light calls 1), replacing a flat count that understated true token cost.
- Handoffs are written BY the XO TO the issue, never pasted through the operator.

## 8. Protocol propagation

Everything the XO commits via API lands on origin, not the operator's disk. Foremen pull at boot, so protocol changes propagate at next boot with zero operator action. This is also a known trap: a booted session runs the protocol it loaded, not the protocol on origin.

## 9. Standing directives (operator-issued)

1. **Occam is the default** (2026-07-14). Simplest thing that can work. Every rule, layer, seat, or script earns its place with evidence. Prefer deleting a rule to adding one. Incremental symptom-patching is the signal to find the root cause.
2. **Zero clicks.** Acceptable operator interruptions: payment/auth gates, deploy gates. Everything else reaching his keyboard is a defect fixed at source, same turn.
3. **Work must not stop when the operator is AFK.** Queues stay full, gates queue on the board rather than blocking lanes, and flights are pre-scoped so lanes run unattended.
4. Fleet-wide changes hit every repo in one pass, active or dormant. The operator never repeats a fleet-wide instruction.

## 10. Failure history (candid, for the reviewer)

- **ATC (predecessor system) died of complexity bias**: lanes, ADR lattices, a central bus, burned subscription limits, three closed tickets total. The current system replaced it.
- **The current system then repeated the pattern at smaller scale.** In one week it accumulated 21 standing lessons, 7 board rules, 3 permission-rail generations, and a full memory cap, without deleting anything until 2026-07-14.
- **Click slavery week.** The dominant operator burden was foremen firing interactive questions at boot (61 AskUserQuestion calls, 34 open questions in 7 days), root-caused not by the XO but by an outside audit seat (Sonnet). The XO had been incrementally patching allowlists while the actual cause was empty queues plus a protocol ambiguity. Fixed at source 2026-07-14 (FOREMAN.md).
- **Divergent state.** Multiple ledger deductions trace to one defect class: asserting paths, folders, or file states from memory or stale notes instead of verifying. Includes recommending deletion of a folder that held 100MB of assets and a private key, and repeated wrong boot paths.
- **Zombie automation.** FLEET-VOICE (SMS gate routing) stacked three unverified dependencies (HTML markers stripped by the write path, a silently failing cron, an unregistered A2P number) and looked alive while routing nothing.
- **Over-building.** A recent build consumed four days that the operator judges could have been roughly 100 lines of code. The XO failed to see both the big picture and the specifics simultaneously.

## 11. Open gaps (review targets)

1. Escalation absorption does not work as designed: foreman prompts still surface to the operator instead of being caught upstream. The FOREMAN.md fixes are hours old and unproven.
2. CA has two clones on disk; which is live is unproven. A fleet normalization audit (every repo under Projects/websites plus gitops: remote, rail, CLAUDE.md, log) is instructed and not yet executed.
3. Context-weight accounting is self-reported by the seat being measured.
4. The ledger measures harm after it lands; nothing yet predicts it.
5. The XO is a single point of judgment with no standing adversarial check on its own operations (this review is the first).

## Review question

Where does this architecture spend operator minutes it should not, and what should be deleted?
