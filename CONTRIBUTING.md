# Contributing to jobq

Bug reports, documentation improvements, and pull requests are welcome.

## Getting started

```bash
git clone https://github.com/akash1047/jobq.git
cd jobq
uv sync
```

Database setup and application commands are documented in
[docs/development.md](docs/development.md).

## Reporting bugs

Open an issue describing:

- What you expected and what happened.
- Steps to reproduce the problem.
- Relevant versions and logs.

Remove credentials, personal information, and sensitive job payloads before posting.

For security vulnerabilities, follow [SECURITY.md](SECURITY.md).

## Proposing changes

For substantial changes, open an issue first to discuss the scope.

Keep pull requests focused. Explain the problem, the resulting behavior, and how you verified the change. Update documentation when behavior or configuration changes.

Add tests for new behavior and bug fixes where practical.

Before submitting:

```bash
uv run ruff check .
uv run ruff format --check .
uv run pytest
```

Target the repository's default branch.

## Conduct

Follow [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).
