# Horus Awards

A stone-design competition platform: members submit stone/marble designs, browse
competitors, vote with 5-star ratings, and comment; administrators moderate the
queue, act as judges, and users get **real-time notifications** over WebSocket.

Built container-first with a clear split:

- **Frontend** — static HTML + MaterializeCSS + vanilla JS/jQuery, no build step.
  See [`frontend/README.md`](frontend/README.md).
- **Backend** — vanilla Node.js (`http`) + built-in `node:sqlite`, **zero runtime
  npm dependencies**; hand-rolled JWT and WebSocket. See
  [`backend/README.md`](backend/README.md).
- **nginx** — serves the frontend and reverse-proxies `/api/*` (incl. the
  WebSocket upgrade) to the backend.

Architecture, data model, and API are documented in
[`implementation-plan.md`](implementation-plan.md).

## Quick start

Requires Docker + Docker Compose, and Node 22+ only if you want to run tests
locally.

```bash
# 1. Vendor the frontend libraries once (MaterializeCSS + jQuery, no CDN).
bash frontend/scripts/fetch-vendor.sh
#    Windows: powershell -ExecutionPolicy Bypass -File frontend/scripts/fetch-vendor.ps1

# 2. Bring up the stack (backend + nginx). SQLite lives in a volume; migrations
#    and seed run automatically on first boot.
docker compose up --build

# 3. Open the app.
#    http://localhost:8080/
```

Seeded on first boot: an admin account (`ADMIN_EMAIL` / `ADMIN_PASSWORD`, default
`admin@horus.test` / `admin12345`), a demo author, the "Horus Awards 2025"
competition, and a handful of approved demo submissions.

> Change `SESSION_SECRET`, `ADMIN_EMAIL`, and `ADMIN_PASSWORD` for anything other
> than local development (set them in the environment or a root `.env`).

## Tests

```bash
# Backend (Node 22+; node:sqlite)
cd backend && npm test

# Frontend (no install needed)
cd frontend && npm test
```

35 tests total (19 backend integration/unit + 16 frontend logic). Both suites use
Node's built-in `node:test` runner.

## Architecture graph (optional, dev/CI)

`graphify` (Python tool in `graphify/`) can generate an architecture graph of the
codebase — see [`implementation-plan.md`](implementation-plan.md) §11.

## Repository layout

```
backend/     vanilla Node API + node:sqlite (README + CLAUDE inside)
frontend/    static MaterializeCSS + vanilla JS site (README + CLAUDE inside)
nginx/       reverse-proxy + static config
graphify/    unrelated Python code-graph tool (dev/CI only)
docker-compose.yml
implementation-plan.md
```
