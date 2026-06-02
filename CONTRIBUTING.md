# Contributing

Thanks for wanting to improve self-evolve. Here's how to help.

## What we're looking for

- **New skills** that encode reusable patterns for Claude Code agents
- **Adherence techniques** — better ways to prevent mid-trajectory drift
- **Skill format improvements** — anything that makes skills more followable
- **Bug fixes** and documentation improvements

## Submitting a skill

A good skill:
- Solves a real, recurring failure mode
- Uses numbered steps (not prose paragraphs)
- Has explicit fallbacks for each step
- Includes trigger phrases in the frontmatter description
- Stays under 500 lines

See `skills/reload-context/references/skill-standards.md` for the full template.

## How to contribute

1. Fork the repo
2. Create a branch (`git checkout -b my-skill`)
3. Make your changes
4. Test the skill in a real Claude Code session — does it actually get followed?
5. Open a PR with a short description of what failure mode the skill prevents

## Style

- Lowercase prose in docs (this project's voice)
- No fluff — say what you mean, stop
- If a sentence doesn't add information, delete it

## Issues

Open an issue if you:
- Found a skill that doesn't get followed reliably (adherence bug)
- Have an idea for a skill but aren't sure how to structure it
- Want to discuss the research or propose a new mechanism

## Code of conduct

Be helpful. Don't be a jerk. That's the whole policy.
