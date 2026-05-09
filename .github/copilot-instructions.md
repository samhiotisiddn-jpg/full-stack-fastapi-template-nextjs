# Copilot Workspace Instructions

## Workspace overview

This repository is a monorepo for a Full Stack FastAPI + Next.js application.
- Backend lives in `backend/`
- Frontend lives in `frontend/`
- The repo uses Docker for local database and optional full-stack development
- Vercel deployment is supported via a separate `vercel-deploy` branch

## Primary conventions

- Backend uses Python, `uv` for environment and dependency management, and SQLModel/Alembic for data models and migrations.
- Frontend uses PNPM, TurboRepo, Next.js v16, and a shared `@workspace` package setup.
- Environment variables are required for both backend and frontend. Use `.env.example` files as the source of truth.
- `docs/running.md` is the canonical local development guide. Prefer it over informal notes.

## Key commands

### Backend

```bash
cd backend
uv sync
source .venv/bin/activate
uvicorn app.main:app --reload
bash ./scripts/test.sh
```

Database setup:

```bash
cd backend
bash scripts/prestart.sh
```

### Frontend

```bash
cd frontend
pnpm install
pnpm run build
pnpm dev
```

### Docker / local services

```bash
docker compose up -d database adminer
```

### Notes

- Use `backend/pyproject.toml` for Python dependency and version constraints.
- Use `frontend/package.json` and `frontend/pnpm-workspace.yaml` for frontend package management.
- The frontend app is in `frontend/apps/web`.
- The backend API is under `backend/app/api/`.

## Important files and directories

- `backend/app/api/` — backend route handlers
- `backend/app/crud.py` — backend database CRUD logic
- `backend/app/models.py` — SQLModel schemas and tables
- `backend/app/core/config.py` — backend settings and environment handling
- `backend/app/core/db.py` — database engine and initialization
- `backend/app/utils.py` — backend helpers, email, token utilities
- `frontend/apps/web/` — Next.js app source
- `frontend/apps/web/package.json` — frontend package scripts and dependencies
- `frontend/apps/web/src/app/api/client-proxy/` — client proxy route for backend cookie auth

## What Copilot should do

- Prefer existing documentation and do not rewrite or duplicate it unless updating outdated content.
- When editing code, preserve the monorepo layout and package boundaries.
- For backend changes, use the `backend` folder and existing `scripts/*.sh` helpers when possible.
- For frontend changes, use the `frontend` package and `@workspace` imports.
- When asked to fix issues, verify against the actual build/test commands listed above.

## What Copilot should not do

- Do not assume a root JavaScript package exists outside `frontend/`.
- Do not change Vercel deployment instructions unless the code change requires it.
- Do not modify `docker-compose.yml` unless the user explicitly asks for Docker environment changes.

## Example prompts

- "Help me fix backend login cookie auth in this FastAPI app."
- "Update the frontend `clientFetch` proxy to preserve cookies when calling the API."
- "Add a new backend API route in `backend/app/api/` and wire it through the frontend client proxy."
- "Use the repo's existing `uv` and `pnpm` workflows to add a new test."

## Next agent customizations to add

- Create a `/create-instruction agent` prompt for backend REST API work, focusing on FastAPI, SQLModel, and Alembic migrations.
- Create a `/create-instruction agent` prompt for frontend work, focusing on Next.js v16, TurboRepo, and `@workspace` package imports.
- Create a `/create-skill` for testing and validation that runs `backend/scripts/test.sh` and `frontend pnpm run build`.
