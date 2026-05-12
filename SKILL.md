---
name: vibepresto
description: Deploy static HTML/CSS/JS sites or framework-exported static builds to VibePresto on WordPress using the published CLI. Use when a user wants to log in, inspect pages, build and verify a frontend app, inspect routes, upload a bundle, create a multi-page deployment, or manage bundle/deployment history. Do not use for wp-admin browser automation, direct REST calls when the CLI covers the task, or SSR hosting.
---

# VibePresto Deploy

Use this skill when the task is to deploy a local static site or static-exported frontend build into the VibePresto WordPress plugin through the supported CLI surface.

## What this skill covers

- Device-style CLI login
- Session inspection with `whoami`
- Page listing, search, creation, status changes, homepage assignment
- Framework-aware `detect`, `build`, `verify`, and `routes inspect`
- Auto-bundling a local static site folder
- Uploading an existing ZIP bundle
- Uploading explicit HTML/CSS/JS/assets files
- Multi-route deployments with existing-page resolution and optional page creation
- Bundle lineage listing, version history, and rollback
- Deployment listing, inspection, promotion, and rollback
- Logging out or revoking a saved session

## Use the CLI, not wp-admin automation

Prefer the published CLI:

`npx vibepresto`

If the CLI repo is checked out locally for development, `node ./bin/vibepresto.js` from that repo is also acceptable.

Do not fall back to browser automation or direct REST calls unless the CLI is clearly blocked or the user explicitly asks for lower-level debugging.

## Recommended workflow

1. Confirm or establish auth:
   - `npx vibepresto whoami --site <site> --json`
   - if not logged in: `npx vibepresto login --site <site>`
2. Decide whether this is a simple static upload or a framework/static-export deployment:
   - simple static folder: use `upload --site-dir`
   - frontend project or prebuilt dist folder: use `detect`, `build` or `verify`, then `routes inspect`, then `deploy`
3. For framework or export-based projects:
   - `npx vibepresto detect --project-dir <dir> --json`
   - `npx vibepresto build --project-dir <dir> --json`
   - or if already built: `npx vibepresto verify --output-dir <dir> --json`
   - `npx vibepresto routes inspect --output-dir <dir> --json`
4. Before first deployment on an unfamiliar project, prefer a dry run:
   - `npx vibepresto deploy --site <site> --output-dir <dir> --dry-run --json`
5. For multi-page or router-based apps, prefer route-manifest deployment:
   - `npx vibepresto deploy --site <site> --output-dir <dir> --create-missing-pages --json`
6. Use bundle/deployment history commands when needed:
   - `npx vibepresto bundles list --site <site> --json`
   - `npx vibepresto bundles versions --site <site> --bundle-id <id> --json`
   - `npx vibepresto bundles rollback --site <site> --page-id <id> --version <n> --json`
   - `npx vibepresto deployments list --site <site> --json`
   - `npx vibepresto deployments show --site <site> --deployment-id <id> --json`
   - `npx vibepresto deployments promote --site <site> --deployment-id <id> --bundle-version-id <id> --json`
   - `npx vibepresto deployments rollback --site <site> --deployment-id <id> --version <n> --json`
7. Prefer `--json` whenever the result needs to be parsed or used by another tool step.

## Upload and deploy modes

### Simple single-page upload

Use when the user already has a plain HTML/CSS/JS site folder with `index.html` at the root.

Example:

```bash
npx vibepresto upload \
  --site <site> \
  --site-dir ./site-folder \
  --name "Landing page" \
  --page-id 2 \
  --json
```

Rules:

- `index.html` must exist at the folder root.
- The CLI validates local references before upload.
- Existing single-page uploads remain valid and should still be used when they fit the task.

### Framework/static-export deployment

Use when the user has a React/Next/Nuxt/Vite/Svelte/TanStack style app that produces static output.

Example:

```bash
npx vibepresto deploy \
  --site <site> \
  --project-dir ./my-app \
  --create-missing-pages \
  --json
```

This flow should:

- detect the project type
- run the static build/export locally
- verify output integrity
- inspect or infer routes
- resolve existing WordPress pages
- optionally create missing pages
- upload the bundle and create/update a deployment

### Prebuilt output directory

Use when the project is already built:

```bash
npx vibepresto deploy \
  --site <site> \
  --output-dir ./dist \
  --dry-run \
  --json
```

## Output handling

- Prefer `--json` for agentic flows.
- Expect:
  - `ok: true` with `data` on success
  - `ok: false` with `error.code`, `error.message`, and `error.details` on failure
- Use JSON mode when you need page IDs, bundle version IDs, route manifests, deployment IDs, target mappings, or dry-run planning output.

## Non-goals

- Do not use this skill for SSR hosting.
- Do not promise that Next.js, Nuxt, SvelteKit, or TanStack server runtimes will run inside WordPress.
- Supported framework flows must end in static/exported HTML plus assets.
- Do not use wp-admin automation except for the human approval step during device login when needed.
