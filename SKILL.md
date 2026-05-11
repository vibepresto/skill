---
name: vibepresto
description: Deploy or upload a static HTML/CSS/JS site to VibePresto on WordPress using the published CLI. Use when a user wants to log in, create or manage pages, manage bundle versions, auto-bundle a local site folder, upload an existing bundle, or assign an uploaded bundle to a page. Do not use for wp-admin browser automation, direct REST calls when the CLI covers the task, or framework/build-pipeline sites.
---

# VibePresto Deploy

Use this skill when the task is to deploy a local static site into the VibePresto WordPress plugin through the supported CLI surface.

## What this skill covers

- Device-style CLI login
- Session inspection with `whoami`
- Page listing, search, creation, status changes, and homepage assignment
- Bundle lineage listing, version history, and rollback
- Auto-bundling a local static site folder
- Uploading an existing ZIP bundle
- Uploading explicit HTML/CSS/JS/assets files
- Logging out or revoking a saved session

## Use the CLI, not wp-admin automation

Prefer the published CLI at:

`npx vibepresto`

If the CLI repo is checked out locally for development, running `node ./bin/vibepresto.js` from that repo is also acceptable.

Do not fall back to browser automation or direct REST calls unless the CLI is clearly blocked or the user explicitly asks for lower-level debugging.

## Recommended workflow

1. Confirm or establish auth:
   - `npx vibepresto whoami --site <site> --json`
   - if not logged in: `npx vibepresto login --site <site>`
2. If the bundle should be assigned to a page, inspect or create the page first:
   - `npx vibepresto pages list --site <site> --json`
   - `npx vibepresto pages search --site <site> --query <text> --json`
   - `npx vibepresto pages create --site <site> --title <title> --status draft --json`
3. Choose the upload mode:
   - static site folder: `upload --site-dir`
   - existing ZIP: `upload --zip`
   - explicit files: `upload --html ... --css ... --js ...`
4. When uploading to a page that already has a bundle, treat the upload as the next version in that same lineage by default.
5. Use bundle versioning commands when needed:
   - `npx vibepresto bundles list --site <site> --json`
   - `npx vibepresto bundles versions --site <site> --bundle-id <id> --json`
   - `npx vibepresto bundles rollback --site <site> --page-id <id> --version <n> --json`
6. After upload, optionally finalize the page:
   - `npx vibepresto pages set-status --site <site> --page-id <id> --status publish --json`
   - `npx vibepresto pages set-homepage --site <site> --page-id <id> --json`
7. Prefer `--json` whenever the result needs to be parsed or used by another tool step.

## Upload modes

### Static site folder

Use when the user has a simple static site folder with `index.html` at the root.

Example:

```bash
npx vibepresto upload \
  --site http://localhost:8000 \
  --site-dir ./site-folder \
  --name "Landing page" \
  --page-id 2 \
  --json
```

Rules:

- `index.html` must exist at the folder root.
- The CLI strictly validates local HTML/CSS/JS references found in `index.html`.
- Remote URLs, `data:` URLs, and anchors are allowed.
- This mode is for plain static HTML/CSS/JS only, not framework or build output workflows.

### Existing ZIP bundle

Use when the site is already prepared as a ZIP:

```bash
npx vibepresto upload --site <site> --zip ./bundle.zip --json
```

### Explicit files

Use when the user has raw files instead of a folder:

```bash
npx vibepresto upload \
  --site <site> \
  --html ./index.html \
  --css ./style.css \
  --js ./app.js \
  --asset ./logo.png \
  --json
```

## Output handling

- Prefer `--json` for agentic flows.
- Expect the CLI to return:
  - `ok: true` with `data` on success
  - `ok: false` with `error.code`, `error.message`, and `error.details` on failure
- Use JSON mode when you need page IDs, page URLs, homepage state, bundle lineage IDs, bundle version numbers, assigned page URLs, or upload metadata like `auto_bundle`.

## Non-goals

- Do not use this skill for framework build pipelines yet.
- Do not use this skill for npm install/build orchestration unless the user explicitly asks for that as separate work.
- Do not use this skill for manual wp-admin interactions except the human approval step during login.
