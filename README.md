# TaskFlow Pro

A project/task management API with JWT auth and real-time updates over WebSockets.
**Problem:** small teams juggle tasks across chat and spreadsheets; TaskFlow centralises
projects and pushes changes to everyone instantly.

**Stack:** Python 3.12 · FastAPI · SQLAlchemy 2 · PostgreSQL (SQLite for dev/tests) · PyJWT · Docker · GitHub Actions

## Quick start
```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
uvicorn src.main:app --reload        # http://localhost:8000/docs
pytest --cov=src                     # 6 tests, ~96% coverage
```
With Docker + Postgres:
```bash
echo "SECRET_KEY=$(openssl rand -hex 32)" > .env
docker compose up --build
```

## API
| Method | Path | Description |
|---|---|---|
| POST | `/api/auth/register`, `/api/auth/login` | Returns a JWT |
| POST/GET | `/api/projects` | Create / list your projects |
| POST/GET | `/api/projects/{id}/tasks` | Create / list (`?status=&limit=&offset=`) |
| PATCH/DELETE | `/api/tasks/{id}` | Update / delete |
| WS | `/api/ws/projects/{id}?token=JWT` | Live `task.created/updated/deleted` events |

Interactive docs (OpenAPI/Swagger) at `/docs`.

## Structure
```
backend/src/{api,models.py,schemas.py,security.py,realtime.py,main.py}
backend/tests/   docs/   .github/workflows/ci.yml   docker-compose.yml
```

## Security notes
PBKDF2-SHA256 password hashing, expiring JWTs, ownership checks return 404 to avoid ID
enumeration, Pydantic validation on all input, non-root Docker user, secrets via env.

## Roadmap
React + TypeScript frontend · Celery/Redis background jobs · Redis caching · Alembic migrations ·
rate limiting · Kubernetes/Terraform · Prometheus/Grafana · CSV/PDF export · i18n
