# jobq

A PostgreSQL-backed job queue and worker system built with Python.

> **Status:** Early development. The capabilities below describe the planned implementation. APIs and configuration may change.

## Planned Features

- Persistent job storage using PostgreSQL
- Concurrent job claiming with `FOR UPDATE SKIP LOCKED`
- Asynchronous execution with bounded concurrency
- Priority-based selection and scheduled jobs
- Retries with exponential backoff and jitter
- Lease-based recovery from worker failures
- Horizontal scaling with multiple picker-worker processes
- REST API for job management

## Tech Stack

| Component | Technology |
|---|---|
| Language | Python 3.13+ |
| Package manager | uv |
| API | FastAPI |
| Database | PostgreSQL |
| Database driver | psycopg 3 |
| Migrations | Alembic and SQLAlchemy |
| Concurrency | asyncio |
| Configuration and validation | Pydantic |
| Testing | pytest |
| Linting and formatting | Ruff |

## Getting Started

### Prerequisites

- Python 3.13+
- [uv](https://docs.astral.sh/uv/)
- PostgreSQL

### Installation

```bash
git clone https://github.com/akash1047/jobq.git
cd jobq
uv sync
```

### Configuration

```bash
cp .env.example .env
```

Set the connection URL for your local database:

```dotenv
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/job_queue
```

Create the database before applying migrations. See [Configuration](docs/configuration.md) for the proposed settings.

### Database Setup

Once the migration environment is implemented and configured:

```bash
uv run alembic upgrade head
```

### Running

Once the application entry points are implemented, start the API:

```bash
uv run uvicorn job_queue.api.main:app --reload
```

Start the combined picker-worker process in another terminal:

```bash
uv run python -m job_queue.picker.main
```

API documentation:

- Swagger UI: http://localhost:8000/docs
- OpenAPI schema: http://localhost:8000/openapi.json

The repository is named `jobq`; the Python import package is `job_queue`.

## Architecture

The API persists job submissions in PostgreSQL. Each picker-worker process atomically claims eligible jobs and executes them through a bounded local executor.

```mermaid
flowchart TD
    Client --> API["FastAPI"]
    API --> DB[(PostgreSQL)]
    Runner["Picker-worker process"] --> DB
    Runner --> Handlers["Job handlers"]
```

Multiple processes coordinate claims using PostgreSQL row-level locking.

Execution is **at least once**. Handlers must make duplicate execution safe, including when an earlier attempt completed external side effects before crashing.

See [Architecture](docs/architecture.md) for ownership, leases, and scaling details.

## Job Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending --> Running: Claim
    Running --> Completed: Success
    Running --> Pending: Retry scheduled
    Running --> Pending: Lease expired; attempts remain
    Running --> Failed: Permanent failure or attempts exhausted
    Completed --> [*]
    Failed --> [*]
```

See [Job Lifecycle](docs/job-lifecycle.md) for retry and recovery behavior.

## Development

Install dependencies:

```bash
uv sync
```

Run checks:

```bash
uv run pytest
uv run ruff check .
uv run ruff format --check .
```

Format code:

```bash
uv run ruff format .
```

Integration tests require a separate PostgreSQL test database.

## Documentation

- [Architecture](docs/architecture.md)
- [Development Guide](docs/development.md)
- [Configuration](docs/configuration.md)
- [Job Lifecycle](docs/job-lifecycle.md)
- [API Reference](docs/api.md)
- [Changelog](CHANGELOG.md)

## Contributing

Bug reports, documentation improvements, and pull requests are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) before contributing.

## Security

Follow [SECURITY.md](SECURITY.md) to report vulnerabilities. Do not disclose vulnerabilities through public issues.

## Code of Conduct

Participants are expected to follow [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## License

A license has not yet been selected.
