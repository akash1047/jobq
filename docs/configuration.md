# Configuration

> Status: Proposed configuration contract. Match these names and defaults in the implementation before release.

Read configuration from environment variables. Local development may also load a `.env` file.

| Variable | Default | Purpose |
|---|---|---|
| `DATABASE_URL` | Required | PostgreSQL connection URL |
| `WORKER_CONCURRENCY` | `4` | Maximum simultaneous jobs per process |
| `POLL_INTERVAL_SECONDS` | `1` | Idle polling interval |
| `LEASE_DURATION_SECONDS` | `60` | Lease duration |
| `LEASE_RENEW_INTERVAL_SECONDS` | `20` | Lease renewal interval |
| `RECOVERY_INTERVAL_SECONDS` | `10` | Expired-lease recovery interval |
| `MAX_ATTEMPTS` | `3` | Default total attempts per job |
| `RETRY_BASE_DELAY_SECONDS` | `5` | Initial retry backoff limit |
| `RETRY_MAX_DELAY_SECONDS` | `300` | Maximum retry backoff limit |
| `LOG_LEVEL` | `INFO` | Logging verbosity |

## Validation

Reject invalid configuration at startup:

- Concurrency and maximum attempts must be positive integers.
- Intervals and durations must be positive.
- Lease renewal must occur before lease expiry.
- Maximum retry delay must be at least the base delay.

Use PostgreSQL time for scheduling and lease comparisons to avoid differences between worker clocks.

## Secrets

Do not commit `.env` files or production credentials. Keep `.env.example` limited to placeholders and local development values.

Avoid logging database passwords or sensitive job payloads.
