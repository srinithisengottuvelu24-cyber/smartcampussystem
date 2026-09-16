# Smart Campus Complaints

A student-friendly campus service desk for reporting, tracking, and resolving college facility complaints.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/smart-campus` — responsive React/Vite application with dashboard, complaint register, create, detail, edit, and delete flows.
- `artifacts/api-server/src/routes/complaints.ts` — complaint REST endpoints and server-side filtering.
- `lib/api-spec/openapi.yaml` — source-of-truth API contract for complaints and dashboard summary.
- `lib/db/src/schema/complaints.ts` — persistent complaint table, enums, and insert validation.

## Architecture decisions

- OpenAPI drives generated client hooks and server validation so the UI and API share one contract.
- Complaint lists support query-based search and filters so the register stays useful as the data grows.
- Dashboard counts come from a dedicated summary endpoint rather than client-only hard-coded values.
- The frontend invalidates related list, detail, and summary queries after every mutation.

## Product

Students can report campus issues with category, location, description, and priority. Staff can search, filter, view, update, assign, resolve, reject, and delete complaints. The overview shows current totals and recent activity.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Run API codegen after changing `lib/api-spec/openapi.yaml`.
- Use the managed API and web workflows instead of starting workspace services manually.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
