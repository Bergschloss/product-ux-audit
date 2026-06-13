# 📋 Multi-Agent UX Audit Pipeline (Tasks Example)

This file demonstrates how to orchestrate a thorough, multi-agent `/product-ux-audit` pipeline. 

> [!IMPORTANT]
> **Core Concept:** To achieve maximum depth and avoid confirmation bias, the codebase is split into distinct **User Flows** or **Module Scopes**. Instead of a human doing this, the LLM agent itself should scan the repository first, identify these scopes, and generate this task list.
> 
> The pipeline executes two independent runs (`runA` and `runB`) for each scope using fresh agent contexts (no shared memory), merges their findings to keep the highest severity, and compiles everything into a master audit report.

---

## 🛠️ Pipeline Rules

1. **Orchestrator Role:** The main agent acts as the coordinator/orchestrator. It reads this task list and sequentially spawns dedicated subagents (with clean, isolated contexts) to perform the actual audits.
2. **Isolation:** Subagents for `runA` and `runB` of the same scope must run independently without shared context to eliminate confirmation bias and catch diverse findings.
3. **Phase Execution:** For each active task, the orchestrator instructs the subagent to trigger the `/product-ux-audit` skill for the specified scope and relevant files.
4. **Execution Mode:** The orchestrator moves through the checklist sequentially, one task at a time, waiting for the active subagent to finish before checking it off and starting the next one.
5. **Progress Tracking:** Update a progress log (e.g., `00_PROGRESS.md`) after each task is completed:
   `[Task ID] | [Scope Name] | CRITICAL: X | HIGH: Y | MEDIUM: Z | DONE`
6. **Merge Strategy:** Once `runA` and `runB` are complete for a scope, merge their final reports into `MERGED.md` (the orchestrator can do this or spawn a merging subagent):
   - Combine shared findings (keep the higher severity of the two).
   - Keep unique findings marked as `[Run A only]` or `[Run B only]`.
7. **Final Consolidation:** Merge all `MERGED.md` files into a single, deduplicated master report (`AUDIT_FINAL.md`), sorting findings by severity: `CRITICAL` ➔ `HIGH` ➔ `MEDIUM` ➔ `LOW`.

---

## 🎯 Example Pipeline Tasks

*Replace these placeholders with the actual flows identified by the LLM during the initial project scan.*

### Output Directory
`Output path: C:\Users\Username\Desktop\_Reports\product-audit_run1\`

### Checklist

- [ ] **01a_auth-onboarding** | App: `web-client` | Files: `src/app/(auth)/*`, `src/api/auth.ts` ➔ `01_auth-onboarding\runA\`
- [ ] **01b_auth-onboarding** | (Same scope as 01a) ➔ `01_auth-onboarding\runB\`
- [ ] **01m_auth-onboarding-merge** | Merge 01a and 01b ➔ `01_auth-onboarding\MERGED.md`

- [ ] **02a_core-feed** | App: `web-client` | Files: `src/components/Feed/*`, `src/hooks/useFeed.ts` ➔ `02_core-feed\runA\`
- [ ] **02b_core-feed** | (Same scope as 02a) ➔ `02_core-feed\runB\`
- [ ] **02m_core-feed-merge** | Merge 02a and 02b ➔ `02_core-feed\MERGED.md`

- [ ] **03a_checkout-billing** | App: `web-client` | Files: `src/app/checkout/*`, `src/api/stripe/*` ➔ `03_checkout-billing\runA\`
- [ ] **03b_checkout-billing** | (Same scope as 03a) ➔ `03_checkout-billing\runB\`
- [ ] **03m_checkout-billing-merge** | Merge 03a and 03b ➔ `03_checkout-billing\MERGED.md`

- [ ] **04a_admin-panel** | App: `admin-dashboard` | Files: `src/app/admin/*` ➔ `04_admin-panel\runA\`
- [ ] **04b_admin-panel** | (Same scope as 04a) ➔ `04_admin-panel\runB\`
- [ ] **04m_admin-panel-merge** | Merge 04a and 04b ➔ `04_admin-panel\MERGED.md`

- [ ] **99_final-merge** | Consolidate and sort all `MERGED.md` findings ➔ `AUDIT_FINAL.md`
