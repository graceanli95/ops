# ops
Status: CI guard active.
Contracts:
- Reads via versioned views: v_*
- Writes via RPC actions: fn_*
- RLS: ON (tenant isolation via jwt_org())
- Performance budgets: CI p95 ≤ 300ms; Prod hot-path p95 ≤ 600ms; ≤3 DB trips; ≤200KB payloads
