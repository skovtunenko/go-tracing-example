# Repository Guidelines

## Project Structure & Module Organization
- `cmd/main.go` bootstraps the Fiber app, tracing, and Redis client.
- `server/` hosts HTTP handlers that expose `/create` and `/get` endpoints.
- `storage/` wraps Redis access for persisting `models/` entities.
- `trace/` contains Jaeger and OpenTelemetry setup helpers.
- Support files: `requests.http` holds sample calls, `docker-compose.yaml` provisions Redis and Jaeger, and `tasks.yaml` documents typical workflows.

## Build, Test, and Development Commands
- `docker compose up -d` launches Redis and Jaeger locally; stop with `docker compose down` when finished.
- `go run ./cmd` starts the note service on `:8080` using the live tracing exporter.
- `go test ./...` executes all Go tests; add `-v` for verbose failure details once suites exist.
- `curl --request POST http://localhost:8080/create ...` mirrors the examples in `requests.http` for manual checks.

## Coding Style & Naming Conventions
- Format code with `gofmt` (or `go fmt ./...`) before committing; keep Go's default tab indentation.
- Package names stay lowercase and match directory names (`storage`, `trace`); exported types and funcs use CamelCase (`NotesStorage`, `InitTracer`).
- Keep HTTP handler files in `server/` focused on request orchestration; push persistence and tracing helpers into their respective packages for clarity.

## Testing Guidelines
- Use Go's standard `testing` package; store unit tests alongside code in `_test.go` files (`storage/redis_test.go`, etc.).
- Prefer table-driven tests for storage logic and Fiber handler scenarios; mock Redis via an in-memory client or use the compose stack for integration runs.
- When touching tracing, assert span attributes with the OpenTelemetry test SDK to prevent regressions in exported data.

## Commit & Pull Request Guidelines
- Follow the existing history: short, imperative messages (`add tasks.yaml...`, `update note retrieval example...`). Aim for <72 characters in the subject.
- Reference issue IDs in the body when applicable and list key testing commands executed.
- Pull requests should summarize scope, call out tracing or schema impacts, and include screenshots or curl output when endpoints change.

## Observability & Local Infrastructure
- Ensure Jaeger is reachable at `http://localhost:16686` before validating spans; the collector endpoint should stay `http://localhost:14268/api/traces` unless configs diverge.
- Keep Redis keys namespaced by UUID (current convention) to simplify trace correlation and avoid collisions across environments.
