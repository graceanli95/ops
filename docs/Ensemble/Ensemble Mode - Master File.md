# Ensemble Mode — Master File

Status: ✅ Active  
Branch policy: `main` protected; required checks: **Run GitLeaks** + **Supabase smoke / ping**  
Environment: Windows 11 25H2; Git for Windows; GitHub CLI; Node.js LTS; npm; pnpm; Supabase CLI 2.54.6  
Supabase (public facts only): project `ensemble_project`; region `us-east-2`; extensions `pg_trgm 1.6`, `pgcrypto 1.3`; function `public.jwt_org()` present.

---

## Runbook A — Fix Supabase CLI on Windows 11 (Verified v2.54.6)

**Goal:** Make `supabase` resolvable on PATH and verified as v2.54.6 (or newer) on Windows 11.

### A. Install the CLI (manual binary)
1. Download from GitHub Releases → asset **`supabase_windows_amd64.tar.gz`**.
2. Extract `supabase.exe` and move it to: `C:\Tools\Supabase\supabase.exe`.

### B. Add a PATHed shim (works everywhere)
Create `%LOCALAPPDATA%\Microsoft\WindowsApps\supabase.cmd` with:
```bat
@echo off
"C:\Tools\Supabase\supabase.exe" %*
