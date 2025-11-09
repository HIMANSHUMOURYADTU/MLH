# Deploying this Vite + React app to Render

This project is a Vite + React static site. You can deploy it to Render as a Static Site. Below are two ways: using the included `render.yaml` (recommended for reproducible infra-as-code) or via the Render dashboard.

## Quick checklist
- Ensure the code is pushed to GitHub (branch `main` or update `render.yaml` branch).
- Confirm `npm run build` creates the `dist/` folder locally.
- Any frontend environment variables must be prefixed with `VITE_` to be exposed to client-side code.

## Option A — Automatic with `render.yaml` (recommended)
1. Commit and push this repo (including `render.yaml`) to GitHub.
2. In Render: New -> Static Site -> Connect your GitHub account and select this repo.
3. Render will read `render.yaml` and create a Static Site with these settings:
   - Build Command: `npm ci && npm run build`
   - Publish Directory: `dist`
   - Branch: `main` (change `render.yaml` if you use a different branch)
4. Add environment variables in the Render Dashboard (see notes below) and trigger a deploy.

## Option B — Manual via Render dashboard
1. In Render: New -> Static Site.
2. Connect the repo and choose the branch (usually `main`).
3. Set these values in the form:
   - Build Command: `npm ci && npm run build`
   - Publish Directory: `dist`
   - Root Directory: (leave blank unless your code is in a subfolder)
4. Add environment variables and deploy.

## Environment variables and Vite
- Vite exposes env vars to client code only if they are prefixed with `VITE_`. For example: `VITE_API_URL`.
- Do NOT commit secret values to the repository. Instead, add them in Render Dashboard -> Environment -> `Add Environment Variable`.
- If you have server-only secrets (for example for a server-side API), keep them separate and do not use `VITE_` prefix.

## Local verification before pushing
1. Install deps: `npm ci` (or `npm install`).
2. Build: `npm run build` — this should create `dist/`.
3. Preview the production build locally: `npm run preview` (this runs `vite preview` and serves `dist/` locally).

## Common issues
- 404s for static assets: ensure `publishPath` is `dist` and that `vite.config.ts` `base` is correct if deploying to a subpath.
- Missing env vars in client code: make sure keys are prefixed with `VITE_`.
- Build fails on CI: check Node version in Render settings; Render uses a default Node version if not specified. You can pin node by adding an `engines` field in `package.json` or adding `NODE_VERSION` in Render environment.

## After deploy
- Visit the URL provided by Render in the service dashboard.
- Check build logs in Render if the deploy fails — they show `npm ci` and `npm run build` output.

If you want, I can also:
- Add a small GitHub Actions workflow for CI build and preview before deploy.
- Extract env var names from your local `.env` file and generate a checklist (I won't include secret values here).
