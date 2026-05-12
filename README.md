# VibePresto Skill

Codex skill for deploying single-page static bundles and framework-exported static builds to a VibePresto-enabled WordPress site.

## Install

```bash
npx skills add vibepresto/skill
```

## What it does

- prefers the published VibePresto CLI
- checks authentication before upload or deploy
- uses framework-aware `detect`, `build`, `verify`, and `routes inspect` flows
- supports route-manifest and multi-page deployments
- uses JSON output for agentic workflows

## Typical workflow

```bash
npx vibepresto whoami --site https://your-site.example --json
npx vibepresto build --project-dir ./my-app --json
npx vibepresto routes inspect --output-dir ./my-app/dist --json
npx vibepresto deploy --site https://your-site.example --output-dir ./my-app/dist --create-missing-pages --json
```

For a simple static page upload, the skill can still use:

```bash
npx vibepresto upload --site https://your-site.example --site-dir ./landing-page --page-id 123 --json
```

The full skill definition lives in [SKILL.md](./SKILL.md).
