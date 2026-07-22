# LCC IT Institutional System — Legacy IT Student Portal

A modern, lightweight institutional web portal for the LCC Information Technology Department.
The **primary module is QR-code-based class attendance**, built on an expandable modular
architecture that will grow into a full student portal (announcements, grades, clearance,
laboratory borrowing, events, and more).

> **Status:** Definition/Design phase — this repository currently contains the complete
> system definition documents that drive implementation.

---

## ✨ Core Capabilities (Phase 1)

- 🔐 **Authentication & RBAC** — students sign in with university credentials via the **OSAS API** (active students only — alumni/inactive rejected); faculty/admin use local accounts; 5 roles, JWT access + rotating refresh tokens, device tracking
- 🏫 **OSAS Integration** — student profiles (ID, program, year, section, status, photo), weekly schedules, and calendar exceptions (holidays, suspensions, makeup classes, special events) synced from the university OSAS API
- 🎓 **Records Management** — students, faculty, subjects, sections, class offerings, schedules, enrollments (with bulk CSV import); adviser & attendance history kept locally
- 📱 **QR Attendance** — one dynamic QR per class/event, regenerating **every minute** (encrypted, time-limited; screenshots expire); optional static QR for events; scan retries allowed with duplicate prevention; server-authoritative Present/Late/Absent computation; manual attendance fallback for technical issues
- 📡 **Live Monitoring** — real-time roster updates via Socket.IO during open sessions
- 📝 **Corrections Workflow** — student-filed requests, faculty/admin review, full audit trail
- 📊 **Reports & Analytics** — session sheets, student histories, class summaries, at-risk lists; PDF / XLSX / CSV export
- 🖥️ **Role Dashboards** — tailored widgets and charts per role
- 🔔 **Notifications** — in-app realtime + transactional email
- ⚙️ **Settings** — academic terms, grace/late thresholds, QR rotation interval, module feature flags

## 🧱 Tech Stack

| Layer | Choice |
|---|---|
| Frontend | React 18 + TypeScript + Vite, Tailwind CSS + shadcn/ui, React Router v6, TanStack Query, Zustand, Recharts, html5-qrcode |
| Backend | Node.js 20 LTS + Express + TypeScript, layered modular monolith |
| Database | PostgreSQL 16 via Prisma ORM |
| Realtime | Socket.IO |
| Validation | zod (shared schemas between client and server) |
| Auth | JWT (15-min access) + rotating refresh tokens in httpOnly cookies |
| Testing | Vitest, React Testing Library, Supertest, Playwright |
| Delivery | pnpm workspaces, ESLint + Prettier, Docker Compose, nginx, GitHub Actions CI/CD |

Choices favor **modern industry standards that stay lightweight**: Vite, a modular monolith
(not microservices), copy-in shadcn/ui components, and Postgres — efficient to build, cheap to host.

## 📁 Definition Documents

| File | Purpose |
|---|---|
| [SystemDefinition.json](SystemDefinition.json) | Master structured definition — project, users, attendance/QR policy, entities, API, stack, deployment |
| [SystemDefinition2.json](SystemDefinition2.json) | Functional spec — permissions matrix, business rules, full entity fields, endpoint list, validation & security rules |
| [SystemDefinition3.json](SystemDefinition3.json) | High-level seed — portal identity, roles, core & future modules |
| [SystemDefinition4.txt](SystemDefinition4.txt) | Q&A requirements source — identity/OSAS integration, attendance & QR policy, data sources, monitoring, reports, security |
| [SystemDefinition5.txt](SystemDefinition5.txt) | Q&A **development & execution** questionnaire — team, timeline, tooling, exact plugin choices, hosting, credentials, CI/CD gates, budget, risks. Answers fill in `development_data/` |

### `definition_data/` — structured per-area definitions

```
definition_data/
├── overview/      system overview, roles & permissions, module registry
├── backend/       API architecture, endpoints, services, realtime, jobs
├── frontend/      app architecture, routing, pages, components, UX standards
├── database/      schema (tables, columns, indexes, constraints), ERD, seeding
├── security/      auth design, OWASP mapping, QR anti-fraud, auditing, policies
├── deployment/    environments, Docker/nginx topology, CI/CD, backups, monitoring
├── testing/       strategy, tooling, coverage targets, key test scenarios
└── maintenance/   operations runbook, backups, updates, support workflows
```

### `development_data/` — Work Breakdown Structure (WBS)

A complete, detailed WBS derived from `definition_data/**` and the `docs/index.html`
prototype: every module broken into work packages and tasks (backend/frontend/database/
security/QA/devops), each carrying its acceptance criteria and a pointer back to the
definition it implements. Structural fields are fully populated; execution fields
(estimate, priority, assignee, dates, status) are placeholders to be filled in using your
answers to [SystemDefinition5.txt](SystemDefinition5.txt).

```
development_data/
└── wbs/
    ├── 00-wbs-index.json                         legend, ID scheme, phase map, critical path
    ├── 01-phase0-foundations.json                monorepo, DB bootstrap, CI, base infra
    ├── 02-phase1-auth-identity.json              auth + OSAS integration
    ├── 03-phase1-people-catalog.json             students, faculty, subjects/sections/offerings, calendar
    ├── 04-phase1-attendance-core.json            QR engine, monitoring, corrections (highest risk area)
    ├── 05-phase1-reports-dashboard.json          reports/exports, role dashboards
    ├── 06-phase1-notifications-settings-audit.json
    ├── 07-phase1-crosscutting-qa-security.json   shared UI shell, continuous testing & security
    ├── 08-phase1-deployment-launch.json          staging, production, backups, go-live
    ├── 09-phase2-modules.json                    Phase 2 stubs (announcements, profile, digital ID)
    ├── 10-phase3-modules.json                    Phase 3 stubs (grades, clearance, lab borrowing)
    ├── 11-phase4-modules.json                    Phase 4 stubs (events, orgs, advising, evaluation)
    └── 12-resourcing-timeline-template.json      team, timeline, budget, risk register — all placeholders
```

## 🚀 Planned Roadmap

1. **Phase 1** — Attendance-first portal (everything above)
2. **Phase 2** — Announcements, extended Student Profile, Digital ID (kiosk scan mode)
3. **Phase 3** — Grades, Clearance, Laboratory Borrowing
4. **Phase 4** — Events, Organizations, Advising, Evaluation

## 🏗️ Planned Repository Layout (implementation)

```
apps/
├── web/        # React SPA (Vite)
└── api/        # Express API (/api/v1)
packages/
└── shared/     # zod schemas + shared TypeScript types
```

## 📄 License

Internal institutional project — all rights reserved by LCC IT Department.
