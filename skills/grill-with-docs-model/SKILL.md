---
name: grill-with-docs-model
description: Pick the model to run on, then run grill-with-docs.
disable-model-invocation: true
---

This skill only pins the model, then hands off to `grill-with-docs` (a skill by Matt Pocock — https://github.com/mattpocock). Grilling is a long interactive session, so its model must be set on the main session — which only the user can do — before the handoff.

1. Ask which model to grill on. Recommend **Opus 4.8** for the deepest interview; also offer Fast Opus, Sonnet 5, Haiku 4.5.
2. Tell the user the switch command for their choice, then wait until they confirm they've switched:
   - Opus 4.8 → `/model claude-opus-4-8`
   - Fast Opus → `/fast`
   - Sonnet 5 → `/model claude-sonnet-5`
   - Haiku 4.5 → `/model claude-haiku-4-5-20251001`
3. Once switched, run the `grill-with-docs` skill — follow its current `SKILL.md` rather than assuming what it contains, so this skill tracks any changes to it.
