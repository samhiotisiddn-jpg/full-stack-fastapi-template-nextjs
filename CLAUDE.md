# Full Stack FastAPI Template (Next.js)

A Turborepo monorepo combining a FastAPI (Python) backend with a Next.js 16 frontend. Server components, server actions, HttpOnly cookie auth, ShadcnUI + TailwindCSS v4.

## Stack

- **Backend**: FastAPI (Python), SQLModel, Alembic, PostgreSQL, `uv` package manager
- **Frontend**: Next.js 16, React, ShadcnUI, TailwindCSS v4, Hey Api
- **Infra**: Docker Compose (local), Vercel (frontend deployment)
- **Monorepo**: Turborepo

## Quick Reference

```bash
# Backend (from backend/)
uv sync                    # install Python dependencies
uv run fastapi dev         # start dev server (port 8000)
uv run alembic upgrade head  # run migrations
uv run pytest              # run tests

# Frontend (from frontend/)
pnpm install               # install Node dependencies
pnpm dev                   # start dev server (port 3000)
pnpm build                 # production build
pnpm lint                  # lint

# Docker (full stack)
docker compose up          # start all services
docker compose down        # stop
```

## Project Structure

```
├── backend/
│   ├── app/              # FastAPI application
│   │   ├── api/          # Route handlers
│   │   ├── core/         # Config, security, auth
│   │   ├── models.py     # SQLModel data models
│   │   └── main.py       # App entrypoint
│   ├── alembic/          # Database migrations
│   ├── pyproject.toml    # Python deps + tool config
│   └── uv.lock           # Locked dependencies
├── frontend/
│   ├── apps/             # Next.js applications
│   ├── packages/         # Shared packages
│   ├── pnpm-workspace.yaml
│   └── package.json
├── docker-compose.yml    # Local development stack
├── docs/                 # Documentation
└── scripts/              # Utility scripts
```

## Database Migrations

```bash
uv run alembic revision --autogenerate -m "description"
uv run alembic upgrade head
uv run alembic downgrade -1
```

Never edit existing migration files after they have been applied.

## Environment Variables

Copy `.env.example` to `.env` and populate before running locally. Never commit `.env` files.

Key variables: `POSTGRES_SERVER`, `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`, `SECRET_KEY`, `FIRST_SUPERUSER`, `FIRST_SUPERUSER_PASSWORD`.

## Deployment

- **Docker**: use the `main` branch.
- **Vercel**: use the `vercel-deploy` branch for frontend-only deployment.
