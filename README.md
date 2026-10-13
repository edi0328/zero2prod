# zero2prod

A newsletter API built while working through [Zero to Production in Rust](https://www.zero2prod.com/). Uses Actix Web, PostgreSQL, and SQLx.

## Current scope

- `GET /health_check` returns an empty successful response
- `POST /subscriptions` accepts a name and email as form data and stores them in PostgreSQL
- Integration tests cover the health check, stored subscriptions, and missing fields
- Configuration comes from YAML files and `APP_` environment variables
- Request and database operations use tracing

Email delivery and subscription confirmation are not implemented yet.

## Run locally

Install Rust with edition 2024 support, Docker, PostgreSQL client tools, and SQLx CLI. On Linux, the Cargo configuration also requires `clang` and `lld`.

```bash
cargo install sqlx-cli --version '~0.8' --no-default-features --features rustls,postgres
./scripts/init_db.sh
export DATABASE_URL=postgres://postgres:password@localhost:5432/newsletter
cargo run
```

The API listens on http://127.0.0.1:8000 with the default local configuration.

```bash
curl -i http://127.0.0.1:8000/health_check
curl -i http://127.0.0.1:8000/subscriptions \
  --data 'name=Edison&email=edison@example.com'
```

Run `cargo test` with PostgreSQL running. Tests create a separate database for each application instance, so the database user needs permission to create databases.

To reuse an existing PostgreSQL server, set `SKIP_DOCKER=true` when running `scripts/init_db.sh`. Match any custom database settings in `configuration/` or through variables such as `APP_DATABASE__PORT`.

## Repository

Routes are in `src/routes`, schema changes in `migrations`, and integration tests in `tests`. Docker and deployment configuration are included. The main Rust workflow currently targets `root-chapter-03-part1` rather than `main`.
