# Ensemble Mode – Master File
_Last Updated: 2025-10-24 09:55 (local)_

> Single source of truth for Ensemble Mode policies, protocols, system state, repo guardrails, and verified runbooks.

---

## 1) Vision & Ground Rules
- **Vision-first cross-check**: Every step references a concrete artifact (screenshot, log, PR, settings page, or CLI output).
- **Painfully Specific Runbook Mode (default)**: Click-by-click instructions; zero ambiguity.
- **Freeze/Resume protocol**: Stop at the first blocking issue; capture evidence; resume from the exact line later.
- **Automatic Data Checks**: Prefer machine-verifiable checks (CLI outputs, API logs, GitHub checks) over descriptions.

---

## 2) Ensemble Policies (active)
- **Memory-First Instructions**: Prefer known facts from Memory Bank before proposing new actions.
- **Tooling Contracts**
  - **GitHub**: Protected `main`; all changes via PR; required checks must pass.
  - **Supabase**: Use **public** ANON key for client-side; **Service Role** only in server/CI contexts (never in client).
  - **Budibase**: Use `v_*/fn_*` views/functions only (no direct table binding).
- **Security**:
  - Secrets live in **GitHub → Settings → Secrets and variables → Actions**.
  - Never commit keys/tokens to the repo.

---

## 3) System Configuration (host)
- **OS**: Windows 11, version **25H2**
- **Latest Update**: 2025-10 Cumulative Update for Windows 11, version 25H2 (KB5070773)
- **Build**: **26200.6901**

---

## 4) Supabase Project (ensemble_project)
- **Project Name**: `ensemble_project`
- **Region**: East US (Ohio) `us-east-2`
- **Project ID**: `iowbsmmpuksltenbgtww`
- **Project URL**: `https://iowbsmmpuksltenbgtww.supabase.co`
- **ANON PUBLIC KEY**:  
  `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Imlvd2JzbW1wdWtzbHRlbmJndHd3Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjEwNjg5MTQsImV4cCI6MjA3NjY0NDkxNH0.PfmsykX8hPra4K5bOE1n0ZyWD1rGsUNvOZUiYpbsKdE`
- **Publishable key**: `sb_publishable_D2ett7OyavCgduvGI39NWQ_oz-exjUt`
- **Legacy JWT secret**:  
  `OxfGxEij+00fW5tIYNs93Rm6MSRmcT5zQUO++JKsK9i4LK6cYpNRuspcJpz4KkNoY7q85pfBCUsO8fvS4shSrQ==`

> **Repo Secrets (GitHub → Actions)**
> - `SUPABASE_PROJECT_URL` = `https://iowbsmmpuksltenbgtww.supabase.co`
> - `SUPABASE_ANON_PUBLIC_KEY` = **ANON** key above
> - `SUPABASE_SERVICE_ROLE_KEY` = service-role JWT (server/CI only)

**Helpers (verified via SQL, Oct 24, 2025)**
- `pg_trgm` **1.6** ✅
- `pgcrypto` **1.3** ✅
- `public.jwt_org()` **present** ✅

---

## 5) Repository (GitHub)
- **Repo**: `graceanli95/ops` (Public)
- **Default branch**: `main`
- **Branches**:
  - `ci/gitleaks`
  - `chore/scaffold` (contains `.gitkeep` seeds: `db/seed`, `db/migrations`, `docs/decisions`, `docs/budibase`, `apps`)
  - Docs branches as created (e.g., `docs/cli-supabase-fix`)

**Branch Protection (main)**
- Require a pull request before merging ✅
- Require status checks to pass before merging ✅
  - **Run GitLeaks** (from `.github/workflows/secret-leak-guard.yml`)
  - **ping** (Supabase smoke job; GitHub UI displays it as `ping`)  
- Require branches to be up to date ✅
- Require linear history ✅
- Require conversation resolution ✅
- Do not allow bypassing settings ✅
- Allow deletions ✅

**CI Workflows**
- `secret-leak-guard.yml` → GitLeaks on PRs ✅
- `ci-supabase-smoke.yml` → REST `GET /auth/v1/health` ping on PRs ✅  
  - Verified Supabase **Logs → API Gateway** show 200s (Oct 23, 2025) ✅

---

## 6) Local Dev Tooling (Windows 11)
- **Node.js LTS** installed.  
- **npm** installed.  
- **pnpm** installed globally.  
- **Git** installed (`Git-2.51.1-64-bit`) ✅
- **GitHub CLI** downloaded (`gh_2.82.0_windows_amd64`) ✅
- **Supabase CLI** installed (see Runbook A below) → `2.54.6` ✅

---

## 7) Memory Bank (key facts)
- Supabase CLI: `C:\Tools\Supabase\supabase.exe` → **2.54.6** (global via shim)
- Shim: `%LOCALAPPDATA%\Microsoft\Windows\Apps\supabase.cmd` → forwards to exe
- PATH includes: `C:\Tools\Supabase`
- Extensions present: `pg_trgm` 1.6, `pgcrypto` 1.3; function `public.jwt_org()` present
- Branch protections and required checks set (GitLeaks + ping)

---

## 8) Status / Verifications
- **VERIFY**: Supabase smoke checks pass in PRs (`ping`) ✅
- **VERIFY**: GitLeaks runs and passes in PRs ✅
- **VERIFY**: Supabase CLI fixed and globally available → `supabase --version` ⇒ **2.54.6** ✅
- **VERIFY**: Supabase API Logs show `GET /auth/v1/health` 200s from GitHub Actions ✅

---

## 9) Appendices / Artifacts

### Runbook A — Fix Supabase CLI on Windows 11 (Verified v2.54.6)
**Format:** Painfully Specific Runbook (Ensemble)  
**Status:** ✅ Complete and verified on this machine

#### Outcome
- `C:\Tools\Supabase\supabase.exe` exists and runs.
- Shim `supabase.cmd` lives in `%LOCALAPPDATA%\Microsoft\Windows\Apps` and forwards to the exe:
  ```bat
  @echo off
  "C:\Tools\Supabase\supabase.exe" %*
