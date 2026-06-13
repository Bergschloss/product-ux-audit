# 🔍 Product UX Audit Skill

> **Professional Standard:** This framework has been validated through 30+ audit runs on a production Turborepo monorepo (Next.js, Fastify, Electron, Prisma/PostgreSQL). It represents a mature, systematic approach to automating QA and product UX auditing, and can be adopted as a standard auditing pipeline for medium-to-large scale commercial projects.

A read-only, multi-phase audit protocol designed for AI coding assistants (such as Antigravity, Claude, or other agentic frameworks). This skill guides the agent to perform a systematic, evidence-based audit of a product's business logic, user journeys, UX heuristics, edge cases, and feature modularity.

## 🌟 What It Does

The skill defines 7 sequential phases of auditing:

* **Phase 0: Flow Map & Scope** - Maps out all screens, routes, APIs, user roles (personas), and external system dependencies.
* **Phase 1: Product Logic Audit** - Searches for logic gaps, contradictory rules between endpoints/views, and inconsistencies in user flows.
* **Phase 2: Heuristic Evaluation** - Evaluates the UI against Nielsen's 10 usability heuristics + Accessibility & cognitive load for non-technical users.
* **Phase 3: Friction, Dead Ends & Edge Cases** - Finds dead-end states, UX friction, network/SSE disconnection recovery behaviors, and unhandled edge cases.
* **Phase 4: User Journey Map** - Maps the primary user journey, touchpoints, feelings, pain points, and product metrics.
* **Phase 5: Architecture & Modularization** - Audits monorepo boundaries, feature encapsulation, type sharing, and frontend/backend validation sync.
* **Phase 6: Competitive Benchmark** - Evaluates the product against user-specified competitors.

---

## How to Use

1. **Install:** Save `SKILL.md` into `~/.claude/skills/product-ux-audit/SKILL.md` (create the folder if needed). Restart your agent session - the skill becomes available as `/product-ux-audit`.
2. **Invoke with a defined scope:** Trigger it for one user flow at a time - e.g. "/product-ux-audit. Scope: auth-onboarding. Files: src/app/(auth)/*, src/api/auth.ts". See Scope Discipline in SKILL.md - never point it at an entire repository in one pass.
3. **Read the report:** Phase reports plus a final summary are written to %USERPROFILE%\Desktop\_Reports\<timestamp>\ - never inside the project repository.
4. **Auditing a whole product?** See "Orchestrating a Large-Scale Audit" below - generate a task list first, then run it one scope at a time.

---

## 📋 Orchestrating a Large-Scale Audit (Multi-Agent Pipeline)

When auditing a large codebase, passing the entire repository to a single agent session at once leads to context overload, low finding depth, and high hallucination rates. 

To solve this, we use a structured **Multi-Agent Audit Pipeline**. The template is provided in [tasks-pipeline-example.md](./tasks-pipeline-example.md).

### How to use this strategy:

1. **Ask the Agent to Scan & Split:** 
   Feed the codebase to your AI assistant and ask it to scan the project files, identify all core **User Flows** or architectural module boundaries, and generate a task list formatted like `tasks-pipeline-example.md`. Let the LLM design the scope partition.
2. **Execute Sequentially:**
   Instruct the agent to execute these tasks **one-by-one**. Keeping each task focused on a single flow (e.g. auth-onboarding) with isolated directory paths ensures the agent stays highly focused and identifies deep, non-obvious issues.
3. **Run Parallel Audits (Run A / Run B):**
   To avoid confirmation bias, start two independent agent sessions (Run A and Run B) with clean contexts for the same scope. 
4. **Merge Findings:**
   Have a merge task after each run pair. The merging agent consolidates findings, taking the highest severity and listing run-specific entries.
5. **Consolidate to Final Report:**
   Run a final task to concatenate all merged flows, deduplicate them, and sort the findings from `CRITICAL` down to `LOW` in a master document (`AUDIT_FINAL.md`).

---

## 💻 Default Technology Stack

The default searches and rules in `SKILL.md` are set up for:
* **Frontend:** Next.js (App Router/Vite), React, TailwindCSS.
* **Backend:** Fastify API, Server-Sent Events (SSE).
* **Database & ORM:** Prisma ORM, PostgreSQL.
* **Desktop Client:** Electron IPC bridges (`ipcMain`, `ipcRenderer`).

---

## 🔄 Custom Stack Adaptation

This skill is framework-agnostic. You can easily adapt it to any other technology stack (e.g., Python/Django, Go, Ruby on Rails, NestJS) by editing `SKILL.md` and modifying:
1. **Search queries (Ripgrep / Grep):** Update directory structures (e.g., check `templates/` or `controllers/` instead of Next.js App Router).
2. **Framework rules:** Replace React hooks/state indicators (e.g., `isLoading`, `isPending`) with patterns native to your frontend framework.
3. **ORM / DB queries:** Replace Prisma method checks (like `prisma.findMany()`) with your ORM's equivalents (like Sequelize `.findAll()` or Django `.objects.all()`).
