# self-evolve

> an agent that can't learn from its mistakes isn't an agent. it's a very expensive grep.

a cowork plugin for building agents that actually improve over time — by encoding learnings
as persistent skills, and staying on track long enough to use them.

backed by actual research. not vibes.

---

## the problem

you've probably noticed this: you spend 30 minutes in a conversation getting claude to do
something exactly right. it figures out the right approach, gets the format correct, finds
the edge case. great session.

next session: starts from zero. no memory of what worked. you're debugging the same thing
again.

there's a second, subtler problem: claude loads a skill at the start of a long task, reads
it, says "got it" — and then quietly stops following it halfway through. not maliciously.
just... drifts. the model's attention moves on and the skill becomes wallpaper.

both of these are real, measured failure modes. not opinions.

---

## the research

this plugin is a direct implementation of findings from:

**"harness updating is not harness benefit: disentangling evolution capabilities in
self-evolving llm agents"**
lin et al., arXiv:[2605.30621](https://arxiv.org/abs/2605.30621), may 2026

the paper ran a cross-factorial experiment: every model acts as agent, every model acts
as skill-writer (evolver), independently. three benchmarks. seven models from 9b to
claude opus 4.6. here's what they found:

### finding 1: any model can write good skills

the spread between the best and worst skill-writer is **at most 3.1 percentage points**
on any benchmark. a 9b open-source model produced skills that were *procedurally
isomorphic* to what opus 4.6 wrote — same steps, different verbosity, same outcome.

**implication:** writing skills from execution traces is a pattern-extraction problem,
not a reasoning problem. don't overthink it. extract the pattern. write the skill.

### finding 2: the bottleneck is using skills, not writing them

after evolution, performance is dominated by the **agent**, not the evolver. swapping
evolvers moves the needle by ~5pp. the agent capability gap is 36pp.

**implication:** invest in the task-solving agent. not the skill-writer.

### finding 3: two ways weak agents fail at harness

**failure mode 1 — activation failure**: qwen3-32b loads a skill in only 25% of
trajectories. strong models: ~96%. the model *knows* the skill exists. it just fails to
issue a clean `load_skill` call. buries it in a multi-step action. environment never
registers it. skill never enters context.

**failure mode 2 — adherence failure**: even when loaded, weak models follow the skill
in only 14% of trajectories. and it gets worse across time — adherence decays **4x more
steeply** for weak models than strong ones. loaded the skill at step 1. by step 20,
winging it entirely.

the paper calls this a "long-horizon instruction-following bottleneck." the model
progressively deprioritizes the loaded skill as the trajectory unfolds. it's not that
it misread the skill — it's that it stops caring about it.

---

## what this plugin does about it

```
self-evolve/
├── skills/
│   ├── improve-urself/     # /improve-urself — encode learnings as skills
│   └── reload-context/     # /reload-context — fix mid-trajectory drift
│       └── references/
│           └── skill-standards.md  # template for writing followable skills
└── CLAUDE.md               # always-on harness adherence instructions
```

### `/improve-urself [optional: topics]`

maps to: **finding 1** (harness-updating is flat — any model can do this)

scans the current conversation for mistakes, corrections, and patterns. checks your
existing skills for overlap. creates a new skill or updates an existing one.

```
/improve-urself
/improve-urself bash error handling
/improve-urself json formatting tool-use
```

the key insight from the paper: you don't need to call a frontier model to write the
skill. the quality of a skill is bounded by what the execution trace reveals —
not by how smart the writer is. just run it. even small learnings compound.

### `/reload-context` (or `/brainwash`)

maps to: **failure mode 2** (harness adherence decay)

re-reads every loaded skill and active instruction. explicitly re-commits to fallback
steps. makes the active harness visible and auditable.

call it when:
- you notice claude improvising around instructions it should be following
- a task is getting long and you want to reset attention
- the output looks right-shaped but something feels off

note: this is not `/brainwash` (that name implies erasure). it's a re-read and
re-anchor. the conversation history stays intact. attention resets.

### `CLAUDE.md` — always-on adherence protocol

maps to: **failure mode 2** + the paper's training recommendations

instead of waiting for you to notice drift, these instructions run in every session:

- state relevant loaded skills before starting multi-step tasks
- re-check alignment at sub-task transitions
- acknowledge fallbacks before starting, not after step 1 fails
- if improvising around a skill → stop and re-read

the paper's recommendation for fixing adherence decay is training-based. for agents
you don't train yourself, this is the next best thing: an always-on instruction that
makes harness adherence explicit and checkable.

### `skill-standards.md`

a template for writing skills that actually get followed, derived from the paper's
failure mode analysis. structured steps beat prose. explicit fallbacks beat "handle
errors." numbered procedures are harder to skip than paragraphs.

---

## install

click the `.plugin` file in the parent directory → "save skill" → restart claude.

or drop `self-evolve.plugin` into your cowork plugins folder manually.

---

## what this plugin doesn't solve

the paper also found that **harness-benefit is non-monotonic** — mid-tier models
benefit most from evolved skills; weak models still struggle even with perfect skills
because they can't reliably invoke or follow them.

if you're running a very weak base model, this plugin will help with skill formatting
(making skills more followable) but it won't fix the underlying capability gap.
the paper's recommendation there: invest in the task-solving agent, not the harness.

also: this plugin doesn't auto-evolve. you have to call `/improve-urself` yourself.
fully automatic skill evolution from conversation traces is a workflow the paper
describes but which requires more infrastructure than a cowork plugin. that's a future
thing.

---

## related

- paper: https://arxiv.org/abs/2605.30621
- code from the paper: https://github.com/A-EVO-Lab/a-evolve/tree/release/harness-evolution
