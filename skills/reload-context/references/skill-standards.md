# skill-standards

a template and guide for writing skills that actually get followed.

the research finding this is based on: models that load skills but don't follow them
have one thing in common — the skills they're reading are ambiguous, prose-heavy, and
don't explicitly state fallbacks. structured skills get followed. vague skills get
summarized once and ignored.

---

## the format

every skill should have:

```markdown
---
name: skill-name
description: >
  [one paragraph: what it does, when to trigger it, specific phrases that invoke it]
---

# skill-name

[one sentence: what problem this solves and why it matters]

---

## step 1: [verb phrase]

[what to do. why. what to watch for.]

## step 2: [verb phrase]

[what to do. if step 2 fails: [explicit fallback].]

...

## what good output looks like

[concrete example]

## what to avoid

[the specific failure mode this skill was created to prevent]
```

---

## rules

**number your steps.** prose procedures get treated as loose guidelines.
numbered steps get followed.

**state fallbacks explicitly.** "if X fails, do Y" not "handle errors gracefully."
the model that reads "handle errors gracefully" will improvise. the model that reads
"if the bash command exits non-zero, print stderr and retry with --verbose" will do that.

**be specific with examples.** "format like this: `{...}`" beats "use JSON format."

**explain the why.** if the reader understands why the rule exists, they apply it
correctly to edge cases you didn't anticipate. if they only have the rule, they
apply it literally and fail on anything slightly different.

**keep it under 500 lines.** longer skills drift. if you need more than 500 lines,
split into a main skill + references/ files and have the skill point to them explicitly.

---

## what makes a skill actually get used

from harness self-evolution research (arXiv:2605.30621):

the bottleneck isn't writing good skills — it's the model invoking them and following
them. the skill format controls the second part. things that help:

- **numbered steps** are harder to skip than paragraphs
- **explicit check-off points** ("before proceeding, confirm X") force re-engagement
- **short sections** reduce mid-trajectory drift
- **fallback chains** prevent the "step 1 = the whole skill" failure mode

things that hurt:

- dense prose (gets summarized and then forgotten)
- implicit fallbacks ("handle errors")
- no examples (model fills in its own interpretation)
- no trigger phrases (model never loads it in the first place)
