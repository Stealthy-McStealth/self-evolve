---
name: improve-urself
description: >
  Extract learnings from the current conversation and encode them as persistent skills.
  Trigger when the user types /improve-urself (with optional topic args like
  "/improve-urself json formatting" or "/improve-urself tool-use bash"), or says things
  like "save that as a skill", "we should remember this", "turn that into a rule",
  "learn from what just happened", "add this to your skills", or "don't make that
  mistake again". Also trigger proactively after a long conversation where multiple
  corrections were made. Always use this skill — don't just summarize the learning in
  chat and move on.
---

# improve-urself

This skill extracts durable learnings from the current conversation and creates or
updates skills so the pattern persists beyond this session.

The core idea: you just ran an experiment. The conversation is the execution trace.
Mine it.

---

## Step 1: Mine the conversation for signal

Scan the conversation for:

- **Mistakes made** — Claude produced wrong output, used the wrong tool, misread intent
- **Corrections given** — user said "no, do it like this", "you should have", "next time"
- **Patterns that worked** — a technique that solved something cleanly, a format the user liked
- **Repeated friction** — anything the user had to clarify more than once
- **Domain-specific knowledge** — facts, conventions, or constraints specific to this project/codebase

If called with topic args (e.g., `/improve-urself bash errors`), focus extraction on
those topics. Otherwise extract the 1–3 most impactful learnings from the whole session.

For each learning, note:
- What happened (the failure or insight)
- What the correct behavior is
- How generalizable it is (just this project? all projects? this type of task?)

---

## Step 2: Check for existing skills to update

Look in the `skills/` directory of this plugin for existing skills with overlapping scope.
Also check any loaded skills in the current session.

If a skill already covers the topic:
- Propose an edit to that skill rather than creating a new one
- Merging is better than fragmentation — don't create a separate skill for every
  correction if they belong together

If no existing skill covers it, proceed to create a new one.

---

## Step 3: Draft the skill content

Use the format in `references/skill-standards.md`. Key principles:

- **Be specific, not vague.** "When running bash commands that might fail, always check
  exit codes and print stderr" is useful. "Be careful with bash" is not.
- **Include the why.** Explain the failure mode being prevented, not just the rule.
- **Include concrete examples.** Show what good output looks like, or contrast with what
  went wrong.
- **Procedural skills need fallbacks.** If the skill prescribes steps, explicitly state
  what to do if each step fails.

---

## Step 4: Write or update the skill file

If creating a new skill:
- Derive a short kebab-case name from the topic
- Create `skills/<skill-name>/SKILL.md` in the plugin directory

If updating an existing skill:
- Read the existing file first
- Add the new learning as a new section or extend an existing one
- Don't remove content unless it directly contradicts the new learning

---

## Step 5: Tell the user what was captured

Brief summary:
- What learning was extracted (one sentence)
- Where it was saved (skill name, new vs. updated)
- Whether it's ready to use in the next session

Don't be verbose. One tight paragraph is enough. The skill file is the artifact —
that's what matters.
