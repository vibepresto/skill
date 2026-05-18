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
- can mark a page as the WordPress homepage or posts page through the CLI
- can assign bundles to individual blog posts and set a default single-post template lineage
- validates WordPress `data-vp-*` placeholders in uploaded HTML
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

For a single blog post, the skill can also use:

```bash
npx vibepresto upload --site https://your-site.example --site-dir ./post-template --post-id 789 --json
npx vibepresto posts set-default-template --site https://your-site.example --lineage-id 12 --json
```

Placeholder example:

```html
<h1 data-vp-source="post" data-vp-field="post_title">Fallback title</h1>
```

The full skill definition lives in [SKILL.md](./SKILL.md).
