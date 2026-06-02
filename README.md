# self-evolve

**Make your Claude Code agent work like a real harness — the one Hermes uses.**

> an agent that can't learn from its mistakes isn't an agent. it's a very expensive grep.

A Claude Code plugin that gives your agent self-evolving harness capabilities: it learns
from mistakes, encodes durable skills, and actually follows them across long trajectories.
The same architecture that makes Hermes effective — applied to your local Claude Code setup.

Backed by peer-reviewed research. Not vibes.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## why this exists

Hermes-style agents don't just run tools. They build up a persistent skill library from
execution traces and stay on-task across long trajectories. That's what makes them
effective over time — they compound.

Out of the box, Claude Code doesn't do this. Every session starts fresh. Skills drift
mid-conversation. You repeat yourself.

This plugin fixes both problems with two mechanisms:

1. **Skill evolution** — extract learnings from conversations and persist them as
   reusable skills (like Hermes's harness updating)
2. **Adherence enforcement** — prevent mid-trajectory drift where the agent loads
   instructions but quietly stops following them

---

## the research

This plugin implements findings from:

**"Harness Updating is Not Harness Benefit: Disentangling Evolution Capabilities in
Self-Evolving LLM Agents"**
Lin et al., arXiv:[2605.30621](https://arxiv.org/abs/2605.30621), May 2026

Key findings:

| Finding | Implication |
|---------|-------------|
| Any model can write good skills (spread: 3.1pp max) | Skill-writing is pattern extraction, not deep reasoning |
| Performance is bottlenecked by the agent, not the evolver | Invest in following skills, not writing them |
| Weak agents fail to activate skills (25% vs 96%) | Trigger phrases and explicit invocation matter |
| Adherence decays 4x faster in weak models over long trajectories | Always-on drift prevention is essential |

---

## what's included

```
self-evolve/
├── CLAUDE.md                          # always-on adherence protocol
├── skills/
│   ├── improve-urself/SKILL.md        # /improve-urself — encode learnings as skills
│   └── reload-context/                # /reload-context — fix mid-trajectory drift
│       ├── SKILL.md
│       └── references/
│           └── skill-standards.md     # template for writing followable skills
└── .claude-plugin/plugin.json
```

---

## skills

### `/improve-urself [optional: topics]`

Scans the current conversation for mistakes, corrections, and patterns. Creates or
updates a persistent skill so the learning survives beyond this session.

```
/improve-urself
/improve-urself bash error handling
/improve-urself json formatting tool-use
```

The key insight: you don't need a frontier model to write the skill. The quality is
bounded by what the execution trace reveals — not by how smart the writer is. Even
small learnings compound across sessions.

### `/reload-context`

Re-reads every loaded skill and active instruction. Explicitly re-commits to fallback
steps. Makes the active harness visible and auditable.

Use it when:
- Claude is improvising around instructions it should be following
- A task is getting long and you want to reset attention
- Output looks right-shaped but something feels off

### `CLAUDE.md` — always-on adherence protocol

Runs every session without explicit invocation:
- State relevant loaded skills before multi-step tasks
- Re-check alignment at sub-task transitions
- Acknowledge fallbacks upfront, not after step 1 fails
- If improvising around a skill → stop and re-read

---

## install

### Claude Code CLI

```bash
claude plugin add Stealthy-McStealth/self-evolve
```

### Manual

Clone this repo into your Claude Code plugins directory:

```bash
git clone https://github.com/Stealthy-McStealth/self-evolve.git ~/.claude/plugins/self-evolve
```

---

## limitations

- **Non-monotonic benefit**: mid-tier models benefit most from evolved skills. Very weak
  models struggle even with perfect skills because they can't reliably invoke or follow them.
- **Not fully automatic**: you call `/improve-urself` yourself. Fully automatic skill
  evolution from conversation traces requires more infrastructure — that's future work.

---

## contributing

PRs welcome. If you've found a pattern that makes skills more followable or adherence
more durable, open an issue or submit a skill.

---

## related

- [Paper: Harness Updating is Not Harness Benefit](https://arxiv.org/abs/2605.30621) — the research this implements
- [A-EVO-Lab/a-evolve](https://github.com/A-EVO-Lab/a-evolve/tree/release/harness-evolution) — code from the paper

---

## license

[MIT](LICENSE)
