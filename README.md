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
