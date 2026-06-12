# 🔍 Product UX Audit Skill

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
