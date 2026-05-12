# VibePresto Skill

Codex skill for deploying simple static sites and framework-exported static builds to a VibePresto-enabled WordPress instance.

## Install

```bash
npx skills add vibepresto/skill
```

## What it does

- prefers the published VibePresto CLI
- checks auth before upload or deploy
- uses framework-aware `detect`, `build`, `verify`, and `routes inspect` flows
- supports route-manifest and multi-page deployments
- uses JSON output for agentic flows

The skill definition lives in [SKILL.md](./SKILL.md).
