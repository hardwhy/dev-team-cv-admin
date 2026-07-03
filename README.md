# dev-team-cv-admin

GitHub Pages host for the **DevTeam admin portal**.

This repo does not contain application source code. The admin app is built in
[`hardwhy/dev-team-cv`](https://github.com/hardwhy/dev-team-cv) and deployed here
by the `Deploy Admin Portal` GitHub Actions workflow.

## Live site

After the first successful deploy:

**https://hardwhy.github.io/dev-team-cv-admin/**

## How deployment works

1. Push to `main` in `hardwhy/dev-team-cv` (or run **Deploy Admin Portal** manually).
2. CI builds `apps/admin-portal` with `VITE_BASE_PATH=/dev-team-cv-admin/`.
3. Static files are pushed to the `gh-pages` branch of this repo.

## GitHub Pages setup

In this repo, go to **Settings → Pages** and set:

- **Source:** Deploy from a branch
- **Branch:** `gh-pages` / `/ (root)`

The `gh-pages` branch is created automatically on the first deploy.

## Required secrets (in `dev-team-cv`)

Configure these under **Settings → Secrets and variables → Actions** in the source repo:

| Type | Name |
|------|------|
| Secret | `ADMIN_DEPLOY_TOKEN` |
| Secret | `VITE_SUPABASE_URL` |
| Secret | `VITE_SUPABASE_ANON_KEY` |
| Variable | `VITE_CLIENT_PORTAL_URL` (optional) |

`ADMIN_DEPLOY_TOKEN` must be a PAT with `repo` scope that can push to this repository.
