---
name: reload-context
description: >
  Re-read and re-anchor to all loaded skills and active instructions. Use when the user
  types /reload-context or /brainwash, or says things like "start fresh", "forget what
  you think you know", "re-read the rules", "you're drifting", "you stopped following
  the instructions", "go back to basics", or "reset". Also trigger proactively if you
  notice yourself improvising around a loaded skill's instructions rather than following
  them — that's the drift this skill is designed to fix.
---

# reload-context

Long conversations cause harness drift. You loaded a skill twenty turns ago and now
you're winging it. This skill fixes that.

The research is clear: adherence to loaded instructions decays over trajectories.
Strong models drift less; all models drift some. This is the manual override.

---

## Step 1: Identify what's loaded

List every skill, instruction, or rule that is currently active in this session:
- Skills explicitly loaded or invoked
- Any CLAUDE.md instructions in scope
- Project-specific conventions established earlier in this conversation
- Any explicit rules the user stated ("always do X", "never do Y")

---

## Step 2: Re-read each one

Don't summarize from memory. Actually re-read each skill file or instruction set.
If a skill file path is known, use Read to load the current content — it may have
been updated since you first saw it.

---

## Step 3: State what you're now following

Write a short, explicit list of the active constraints and procedures:

```
reloaded. currently following:
- [skill-name]: [the key procedure in one line]
- [rule from user]: [the rule]
- [project convention]: [the convention]
```

This makes the reload visible and auditable. The user can correct anything that's
wrong or missing.

---

## Step 4: Commit to the fallbacks

For any procedural skill that has fallback steps: explicitly acknowledge them.
"If step A fails, I will try step B before giving up" — not just "I will follow the skill."

The most common failure mode is treating step 1 as the whole procedure. Reloading
means recommitting to the full procedure including fallbacks.

---

## What this doesn't do

This skill re-anchors to existing loaded instructions. It doesn't:
- Create new skills (use `/improve-urself` for that)
- Erase conversation history
- Change your base behavior

It's a deliberate attention reset. That's all it needs to be.
