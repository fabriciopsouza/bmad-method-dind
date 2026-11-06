# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A devcontainer + BMAD-METHOD v6 scaffold. Nothing here is application code: it is an environment
(`.devcontainer/`, `.github/workflows/`) plus an AI-agent methodology installation (`_bmad/`,
`.claude/skills/bmad-*`). Application projects live in their own repositories, never inside this one.

## BMAD v6 layout

| Path | Content | Edit? |
|---|---|---|
| `_bmad/core/`, `_bmad/bmm/`, `_bmad/_config/`, `_bmad/scripts/`, `_bmad/render/` | Installer-managed core and `bmm` module | No: regenerated on every install |
| `_bmad/config.toml` | Team install answers | No: re-run the installer |
| `_bmad/config.user.toml` | Personal answers (name, language) | No; git-ignored |
| `_bmad/custom/config.toml` | Team overrides, never touched by the installer | Yes (`bmad-customize` skill) |
| `.claude/skills/bmad-*` | Skills exposed to Claude Code | No: regenerated |
| `_bmad-output/` | Workflow artifacts (planning, implementation) | Output; git-ignored |

`bmad-help` recommends the next skill from the project state. Typical chain: `bmad-product-brief` →
`bmad-prd` → `bmad-ux` → `bmad-architecture` → `bmad-create-epics-and-stories` → `bmad-sprint-planning` →
`bmad-build` → `bmad-code-review` → `bmad-retrospective`.

## Commands

```bash
# Update BMAD (non-interactive); restart Claude Code afterwards, skills load at startup
npx bmad-method@latest install --directory . --modules bmm --tools claude-code --action update --yes
```

Node 20+ is required.

## Relationship to the user's global framework

The user's global agent framework (`~/.claude/skills/`: pmo, discovery, architect, developer, qa-critic, …)
is also active here and overlaps BMAD. Pick one orchestrator per block and say which: BMAD when the
deliverable is its artifact chain (brief → PRD → architecture → epics/stories → sprint); the global
framework for everything else.

Checklists that people execute in a system, including those in BMAD stories, are xlsx files built by a
versioned script, following Part C of the `output-format` skill of the global framework.

## GitHub Actions

`.github/workflows/claude.yml` responds to `@claude` in issues/PR comments; `claude-code-review.yml`
reviews every PR. Both need the `CLAUDE_CODE_OAUTH_TOKEN` repository secret.

## Remotes

`origin` = `fabriciopsouza/bmad-method-dind` (public fork); `upstream` = `vipasane/bmad-method-dind`.
