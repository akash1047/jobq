# Development

## Requirements

- Python 3.13+
- uv
- PostgreSQL

## Setup

```bash
git clone https://github.com/akash1047/jobq.git
cd jobq
uv sync
cp .env.example .env
```

Create a local PostgreSQL database named `job_queue` and configure `DATABASE_URL` in `.env`.

The application and Alembic must load this configuration.

## Database migrations

Once the migration environment is configured:

```bash
uv run alembic upgrade head
```

Create a migration after changing database models:

```bash
uv run alembic revision --autogenerate -m "describe schema change"
```

Review generated migrations before applying them.

## Running

Once the entry points are implemented:

```bash
uv run uvicorn job_queue.api.main:app --reload
```

In a separate terminal:

```bash
uv run python -m job_queue.picker.main
```

The picker entry point runs the combined picker-worker process.

## Checks

```bash
uv run ruff check .
uv run ruff format --check .
uv run pytest
```

Format code with:

```bash
uv run ruff format .
```

## Tests

Keep isolated tests in `tests/unit/` and PostgreSQL-backed tests in `tests/integration/`.

Use a separate test database. Never run integration tests against production.

Cover concurrent claims, ownership checks, retries, and recovery from expired leases.

## Package layout

The repository is named `jobq`. The Python import package is `job_queue`, located under `src/job_queue/`.

Configure the project as an installable package so `uv run` can import it.
