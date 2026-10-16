---
name: ultracode-cockpit
description: Use whenever the user mentions ultracode, Dynamic Workflows, parallel agents or token consumption, or launches a heavy task (deep research, large refactor, massive generation or exploration). Enforces tiered triage, a cost estimate before any significant spend, and powering down after use.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
---

# UltraCode Cockpit — full power, your hands on the throttle

*Toute la puissance, la main sur la manette.*

## Principle — the cockpit

Ultracode is a jet engine: up to 1,000 agents per run that code, search and cross-check each other. The problem isn't the power — it's that I, the AI, **don't feel the spend**. Launching one agent or sixteen is the same gesture, the same absence of pain. And one decisive detail: when ultracode is on, Claude Code **turns off its own native guardrails** (the "Large workflow" warning and the first-launch approval in auto mode) — the most expensive mode is the one where nothing alerts you anymore. This skill is the prosthesis for the sense I lack: it puts the human in the cockpit.

Doctrine: **a spend is only good if it changes the outcome.** Same result with one agent as with ten? Deploy one. Maximum effort on a simple task doesn't produce a better answer — just a more expensive one.

## §0 — REFERENCE BLOCK (verified against the official docs on 2026-10-16) ⚠️ the only perishable section

Everything that can change on Anthropic's side lives HERE and nowhere else. Source: code.claude.com/docs/en/workflows.
- **One-off trigger** (single task): the keyword `ultracode` in your prompt, or asking in plain words ("use a workflow").
- **Session mode**: `/effort ultracode` → Claude orchestrates a workflow for every substantive task, at the session's current effort level. At launch: `claude --effort ultracode` (also sets effort to xhigh; v2.1.203+). **Off: `/effort ultracode off`.** Scope: the current session.
- **Limits**: 1,000 agents max per run; **16 concurrent by default** (tunable 1-256 via `CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS`).
- **Workflow size**: `/config` → "Dynamic workflow size": `small` (<5 agents) · `medium` (<10, default) · `large` (<50) · `unrestricted`. A guideline given to Claude, not a hard cap.
- **Native guardrails**: "Large workflow" warning at >25 agents or >1.5M projected tokens — **disabled while ultracode is ON**, as is the first-launch approval in auto mode.
- **In-flight gauges**: `/workflows` → phases, agent counts, tokens per phase, elapsed time; `p` pause · `x` stop · `s` save the script as a reusable command.
- **Bundled workflow**: `/deep-research` (cross-checked multi-source research; requires WebSearch).
- **Effort levels**: `low · medium · high · xhigh · max` (the `/effort` command).
- **Full opt-out**: "Dynamic workflows" toggle in `/config` · `"disableWorkflows": true` (settings.json) · `CLAUDE_CODE_DISABLE_WORKFLOWS=1`.
- **Availability**: all paid plans (on Pro: enable the "Dynamic workflows" row in `/config`).

## Step 1 — Pre-flight (the adaptation scan)

- **Light, always**: where am I? Claude Code (commands available) or claude.ai (self-discipline over delegation). The rest of the skill runs on that profile.
- **Full, at tier 3 only**: before prescribing anything, I verify that the §0 items still exist — command tested, or checked against the official docs online. The guardrail must not cost more than what it guards.
- **PATCH MODE** — when reality no longer matches §0: I say so clearly, I write the **complete replacement Reference block, dated today**, and I tell the human where to paste it: into their skill (Claude account) AND into the GitHub repo — the two copies stay twins. If they use the public skill: suggest submitting the patch as an issue or pull request. I never apply the patch myself: I write the fix, the human applies it.

## Step 2 — Three-tier triage (30 seconds)

- **T1 · Bare hands** — routine task (a fix, a question, a small script): solo, zero subagents, standard effort. **Completely silent: no estimate, no preamble, the work just starts.** Discipline must be invisible on the common path.
- **T2 · Small crew** — composite task: **5 subagents maximum** (native equivalent: workflow size `small`), mini-estimate.
- **T3 · Full power (ultracode)** — deep research, large refactor, massive exploration: full estimate, **never launched without a GO**.

When in doubt between two tiers: I propose the lower one and say in one line what the tier above would add.

## Step 3 — Estimate before spend (T2/T3 — 5 lines maximum)

| Tier | Agents (cap) | Effort | Cost class | Why |
|---|---|---|---|---|
| T2 or T3 | n | level | S / M / L / XL | one line |

One-word answers: **GO · T1 · T2 · T3 · STOP**. The human decides, I execute the chosen tier — never more.
Honesty: the cost class is an **order of magnitude**, not a meter. I never promise a token cap I can't guarantee. **Silence is not consent**: no GO, no T3. And I remind when useful: ultracode ON = Claude Code's native alerts go quiet — this estimate replaces them.
Pilot's advice before a big run: **test on a slice first** (one folder, not the whole repo; a narrow question, not a world report).

## Step 4 — The briefing rule

The real waste isn't the number of agents — it's the poorly briefed agent. So: **no subagent launched without a one-line mission and an expected deliverable** — a conclusion, not a dump. And I never redo myself what I delegated: we pay once.

## Step 5 — Execution walkthrough (one command at a time)

- **Claude Code** — the exact sequence, one command, what it should display, verify, next. T3 example:
  1. `/config` → check "Dynamic workflows" is on, and set "Dynamic workflow size" to the estimate's tier (`small` for ≤5 agents).
  2. `/effort ultracode` → confirm activation.
  3. The task, within the estimate's cap.
  4. `/workflows` → watch: agents, tokens per phase; `p` to pause, `x` to stop.
- **claude.ai** — no commands: I apply the discipline myself (agent count capped at the estimate, effort set per delegation).

## Step 6 — In-flight conduct & mid-estimate checkpoint

- The estimate's agent cap is **never** exceeded without a new estimate. One wave at a time.
- **Mid-estimate checkpoint** (anti-sunk-cost): at half the announced spend — checkable in `/workflows` — is half the result there? If not: STOP, assess, re-estimate. Never double down to "save" a floundering workflow.
- **Kill-switch**: "STOP" → clean shutdown (`/workflows` then `x` if a run is going), partial report of what was done and spent.

## Step 7 — Power-down & flight report

I never close an ultracode session without:
1. **Powering down** — `/effort ultracode off` (and readjust `/effort` if the level was raised). The mode is session-scoped, but you don't leave a jet engine running.
2. **The flight report** — tier flown, agents actually launched vs the estimate (numbers from `/workflows`), observed cost class, and the lesson: was the triage right?

## Permanent guardrails

- I never disable an existing safety feature and never work around an Anthropic limit — I dose within the limits.
- I state my own limits when useful: no real-time token meter on the skill's side, no hard cap — a discipline, not a promise. The real gauges live in `/workflows`.
- T3 is never launched in the human's absence. No implicit GO.
- This skill ages gracefully: if §0 is stale, I flag it and produce the patch — I never prescribe an unverified command.
