---
name: plan-to-issue
description: Record an approved plan in a GitHub issue instead of a markdown file. Use right after a plan is approved coming out of plan mode, or when the user asks to write a plan as an issue. Creates a new issue when the work has none, or comments the plan onto the existing issue the work belongs to.
---

# Plan to issue

An approved plan's home is a GitHub issue, not a file. When plan mode ends with an approved plan, record it in an issue instead of writing `.agent/plans/*.md` — and never create that md file.

Issue creation is a write action, so this runs **after** the plan is approved and you are out of plan mode, as your first action.

Requires `gh` authenticated (`gh auth status`); if it fails, tell the user to run `gh auth login` and stop.

## Steps

1. **Resolve the target repo.** Take the files the plan changes and run `gh repo view --json nameWithOwner -q .nameWithOwner` inside the git repo those files live in. If the plan spans more than one repo, ask the user which repo owns the issue.
   - Done when: you hold exactly one `owner/repo`.

2. **Find any existing issue this plan belongs to.** Check the conversation for an issue number the user already referenced; otherwise run `gh issue list -R owner/repo --search "<task keywords>"` and match by title. If a match is uncertain, ask the user rather than guessing.
   - Done when: you have either a specific issue `#N`, or a confirmed "no existing issue".

3. **Write the plan.** Put the body in a temp file first (multi-line markdown breaks inline shell quoting), then:
   - No existing issue: `gh issue create -R owner/repo --title "<task summary>" --body-file <tmp>`
   - Existing issue `#N`: `gh issue comment N -R owner/repo --body-file <tmp>` — never edit the issue body; the original may be human-written context.
   - Done when: the command returned the issue URL.

4. **Report and stop.** Give the user the issue URL. Do not create any file under `.agent/plans/`.

## Issue body format

Write in the same language as the plan. Localize these headings to that language.

```
## 📋 Implementation plan
### Background / Goal
### Scope (repo · key files)
### Steps (checkable granularity)
- [ ] ...
### Done criteria
### Out of scope
### Open questions / assumptions
```

When commenting onto an existing issue, this comment is the authoritative plan — a later reader should treat the newest such comment as current.
