# 🛠 DevTeam — Admin Portal

The CMS dashboard for managing all portfolio content. Fully protected — only authenticated admins get in. 🔐

> ℹ️ **This repo only hosts the built site.** The application source lives in
> [`hardwhy/dev-team-cv`](https://github.com/hardwhy/dev-team-cv) under `apps/admin-portal`
> and is published here (to the `gh-pages` branch) by the `Deploy Admin Portal` workflow.

**Live site:** **https://hardwhy.github.io/dev-team-cv-admin/**

---

## ✨ What's inside

| Page | What it does |
|------|-------------|
| 📊 **Dashboard** | Stats overview + recent contact submissions |
| 👥 **Team** | Full CRUD for team members — photo upload, colors, skills, social links |
| 🚀 **Projects** | Full CRUD for projects — thumbnail upload, tech chips, team assignment |
| 🖼 **Media** | Browse, upload, copy URL, and delete files from Supabase Storage |
| 📬 **Contacts** | View and delete contact form submissions |
| ⚙️ **Settings** | Edit team name, tagline, and copyright name |

---

## 🚀 Running locally

Clone the source repo and run from the monorepo root:

```bash
git clone https://github.com/hardwhy/dev-team-cv.git
cd dev-team-cv
npm install
npm run dev:admin
```

Opens at **http://localhost:4300** 🎉

---

## 🔑 Environment variables

Create `apps/admin-portal/.env` (copy from `.env.example`):

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

---

## 🔐 Authentication

Login is handled via **Supabase Auth**. Create an admin user in your Supabase project:

1. Go to **Authentication → Users** in your Supabase dashboard
2. Click **Add user** and set an email + password
3. That's it — log in at `http://localhost:4300/login`

---

## 🗄 Database setup

Run these SQL files in your Supabase SQL editor **in order**:

```
supabase/migrations/001_initial_schema.sql           ← tables, RLS, triggers
supabase/migrations/002_settings.sql                 ← site settings table
supabase/migrations/003_contact_links_and_rls_fix.sql ← RLS policy fix (required!)
supabase/migrations/004_storage_policies.sql         ← storage upload policies (required!)
supabase/seed/001_seed.sql                           ← initial team members
```

---

## 🖼 Storage setup

Create two buckets in **Supabase → Storage**:

| Bucket | Access | Used for |
|--------|--------|---------|
| `team-members` | Public | Profile photos |
| `projects` | Public | Thumbnails & galleries |

---

## 🏗 Build for production

```bash
npm run build:admin
# Output → apps/admin-portal/dist/
```

---

## 🚀 Deployment (this repo)

The main repo's GitHub Pages site is used by the **client portal**, so the admin
portal deploys here — to **[hardwhy/dev-team-cv-admin](https://github.com/hardwhy/dev-team-cv-admin)** —
via `.github/workflows/deploy-admin.yml` in the source repo.

**How it works**

1. Push to `main` in `hardwhy/dev-team-cv` (or run **Deploy Admin Portal** manually).
2. CI builds `apps/admin-portal` with `VITE_BASE_PATH=/dev-team-cv-admin/`.
3. Static files are pushed to the `gh-pages` branch of this repo.

**1. Enable GitHub Pages** on this repo: **Settings → Pages → Deploy from a branch**,
branch `gh-pages` / `/ (root)`. The `gh-pages` branch is created automatically on the first deploy.

**2. In the source repo**, add these under **Settings → Secrets and variables → Actions**:

| Type | Name | Value |
|------|------|-------|
| Secret | `ADMIN_DEPLOY_TOKEN` | Fine-grained PAT on `hardwhy/dev-team-cv-admin` with **Contents: Read and write** (push `gh-pages`) and **Pages: Read and write** (trigger site build). Classic PAT: `repo` scope also works. |
| Secret | `VITE_SUPABASE_URL` | your Supabase URL |
| Secret | `VITE_SUPABASE_ANON_KEY` | your Supabase anon key |
| Variable | `VITE_CLIENT_PORTAL_URL` | (optional) e.g. `https://hardwhy.github.io/dev-team-cv/` |
| Variable | `ADMIN_PAGES_REPO` | (optional) override target; defaults to `hardwhy/dev-team-cv-admin` |

**3. Push to `main`** (or run the workflow manually). The admin build is published to
**https://hardwhy.github.io/dev-team-cv-admin/**.

---

## 🧰 Tech stack

`React` · `Vite` · `TypeScript` · `Tailwind CSS` · `Supabase Auth` · `React Router v6` · `React Hook Form`
