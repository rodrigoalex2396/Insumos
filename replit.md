# SolarSIM — Registro de Chips

App de registro y gestión de chips SIM instalados en plantas fotovoltaicas.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/chip-registry run dev` — run the frontend (port 21979)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Tailwind CSS, shadcn/ui, Wouter, TanStack Query
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — OpenAPI spec (source of truth)
- `lib/db/src/schema/chips.ts` — Chip table schema (Drizzle)
- `artifacts/api-server/src/routes/chips.ts` — Chip API routes
- `artifacts/chip-registry/src/` — React frontend
  - `pages/dashboard.tsx` — Dashboard with stats
  - `pages/chips-list.tsx` — Chip registry table
  - `pages/chip-form.tsx` — Register new chip form
  - `pages/chip-detail.tsx` — Detail + edit + delete

## Architecture decisions

- OpenAPI-first: spec defines contract, codegen produces hooks and Zod schemas
- Frontend accesses API via `/api` path prefix (shared reverse proxy)
- Stats computed in-memory from DB results (no extra SQL aggregation needed at this scale)

## Product

Registro de chips SIM de plantas fotovoltaicas con:
- Dashboard de métricas (total chips, breakdown por FV, tipo SIM, cobertura)
- Listado completo con búsqueda y ordenamiento
- Formulario de registro con validación
- Vista detalle con edición y eliminación

## Chip fields

- Nombre de planta
- FV (identificador fotovoltaico)
- Tipo SIM (Nano SIM, Micro SIM, eSIM, SIM estándar)
- Número de serie
- Cobertura (Alta / Media / Baja)
- Notas (opcional)

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Always run `pnpm --filter @workspace/api-spec run codegen` after changing `openapi.yaml`
- Always run `pnpm --filter @workspace/db run push` after changing schema files

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
