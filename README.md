# UltraCode Cockpit

**Full power. Your hands on the throttle.**

*UltraCode Cockpit* is a skill for Claude (claude.ai & Claude Code) that governs **ultracode / Dynamic Workflows** — the mode that can launch up to 1,000 agents per run. It doesn't bridle the power: it puts the human in the cockpit, with tiered triage, an **estimate before any significant spend**, in-flight caps, and a mandatory power-down after use.

Built by the same human–AI duo as [Avant-Garde](https://github.com/Alexandre-LPZ/avant-garde): the shield guards what you build, the cockpit guards what you spend.

## Why

Dynamic workflows are spectacular — and expensive by design: every agent carries its own context, and runs count toward your plan's usage limits. Two facts make a governor genuinely necessary:

1. **The AI doesn't feel the spend.** Launching one agent or sixteen is the same gesture. Nothing resists. During our test campaign, a mundane market-study prompt — no "ultracode" anywhere — spawned 3 agents and burned **~1.12M tokens** with zero cost arbitrage.
2. **Ultracode turns off the native guardrails.** Per [Anthropic's own documentation](https://code.claude.com/docs/en/workflows), while ultracode is on, **three** checks go quiet: the "Large workflow" warning (>25 agents or >1.5M projected tokens), the first-launch approval in auto permission mode, and the session's concurrent subagent limit for Agent-tool subagents. The most expensive mode is exactly the one where nothing alerts you anymore.

UltraCode Cockpit fills that gap with a discipline, not a lecture. v1.0.1 was hardened by a live test campaign (trigger, obedience, pressure resistance, kill-switch, patch mode, real Claude Code session) — the fixes it forced are in the skill. v1.1 taught it to *propose* the mode itself when a task truly deserves it (never for small tasks). v1.2 taught it to install its own lookout line, with your consent.

## What it does

1. **Pre-flight** — detects the environment (Claude Code vs claude.ai) and, before a full-power run, verifies its reference facts against reality. Anthropic changed something? The skill **writes its own patch** (a dated replacement block) and tells you where to paste it.
2. **Three-tier triage** — T1 bare hands (silent, zero overhead on routine work) · T2 small crew (≤5 subagents) · T3 full power (ultracode).
3. **Estimate before spend** — a 5-line table (agents cap, effort, cost class, why); you answer in one word: GO / T1 / T2 / T3 / STOP. **No GO, no launch. Silence is not consent.**
4. **The briefing rule** — no subagent without a one-line mission and an expected deliverable; never pay twice for delegated work.
5. **In-flight conduct** — caps honored, one wave at a time, a **mid-estimate checkpoint** against sunk-cost spirals, a kill-switch.
6. **Power-down & flight report** — `/effort ultracode off`, then: tier flown, agents vs estimate, cost class observed, lesson learned.

Doctrine, in one line: **a spend is only good if it changes the outcome.**

## The volatile facts live in ONE block

Dynamic Workflows is a research preview — commands and limits will move. Everything perishable is isolated in the skill's **§0 Reference block**, dated and verified against the [official docs](https://code.claude.com/docs/en/workflows). When drift is detected, the skill generates the replacement block; updating it is one paste (your account + this repo). Patches welcome as issues or PRs — the community is the watchtower.

## Install

**On claude.ai (web/desktop):** Settings → Capabilities/Skills → *Create a skill* → name it `ultracode-cockpit`, paste the description and the content of [`en/ultracode-cockpit/SKILL.md`](en/ultracode-cockpit/SKILL.md) (or the French version), save.

**On Claude Code:** copy the folder of your language into your skills directory:

```bash
# English
cp -r en/ultracode-cockpit ~/.claude/skills/
# Français
cp -r fr/ultracode-cockpit ~/.claude/skills/
```

On Claude Code, **prefer the file install above** over pasting the skill into your claude.ai account: in our tests, account-synced skills were listed to the model by *name only* (no description), which makes automatic triggering unreliable, while file-installed skills keep their trigger description.

## Activation — the honest part

Skill triggering is probabilistic: the model reads the skill's description and decides. Three ways to fly, from zero-setup to bulletproof:

1. **Say the word (zero setup).** Mention ultracode, workflows, subagents or your token spend — the cockpit is designed to light up. This is the core promise, and what the skill's description targets.
2. **Call it by name (zero setup, always works).** "Use ultracode-cockpit" / « applique ultracode-cockpit ». One sentence, guaranteed boarding.
3. **The lookout line (recommended for heavy users — since v1.2, the skill installs it for you).** One line in a layer Claude re-reads every session. On load, the skill checks whether it's in place and, if it's missing, offers to set it up — one GO and it's done:
   - **Claude Code**: with your GO, the skill adds this line to your global `~/.claude/CLAUDE.md` itself (created if missing, never overwritten):
     > *Before launching any subagent (Agent tool) or workflow, load the ultracode-cockpit skill and apply its triage — estimate per tier, T1 stays silent.*
   - **claude.ai / app**: skills can't edit account settings, so it hands you that same line, ready to paste into your personal instructions (Settings → Profile), plus a memory note as backup.

   This is what makes the cockpit catch the heavy tasks that never say "ultracode" — like the 1.12M-token study above. Field note: that exact CLAUDE.md line fired on first try (fresh session, zero trigger words in the prompt → estimate displayed before any agent launched).

We say it plainly because we measured it: in environments that list skills by name only, no description can fire — that's a platform behavior, not a knob this skill can turn. The lookout line is the fix until it changes.

## Languages

- 🇫🇷 [`fr/ultracode-cockpit/SKILL.md`](fr/ultracode-cockpit/SKILL.md) — version française (originale)
- 🇬🇧 [`en/ultracode-cockpit/SKILL.md`](en/ultracode-cockpit/SKILL.md) — English version

## See also

- **[Avant-Garde](https://github.com/Alexandre-LPZ/avant-garde)** — the companion skill: 6 foundation documents + a 10-point security gate before any vibe-coded app ships. Shield first, cockpit second, and your project is both safe and solvent.

## Credits

Built by a human–AI duo: a non-developer entrepreneur and Claude, as equal partners. The need was spotted thanks to **@ia_irl**'s explainer on ultracode; every fact in the reference block was then verified against Anthropic's official documentation.

## License

[MIT](LICENSE) — use it, fork it, improve it. If it saved your token budget, a ⭐ helps others find it.
