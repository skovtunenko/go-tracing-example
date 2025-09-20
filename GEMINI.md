# Project Overview

This project is a "Notes service" written in Go. It provides a simple API for creating and retrieving notes. The service uses Redis for storage, the Fiber web framework for its API, and OpenTelemetry for distributed tracing.

## Key Technologies

*   **Go:** The programming language used for the application.
*   **Redis:** In-memory data store used for note storage.
*   **Fiber:** A Go web framework used to build the API.
*   **OpenTelemetry:** Used for distributed tracing to monitor and debug the application.
*   **Docker:** The `docker-compose.yaml` file suggests that the project is intended to be run with Docker.

# Building and Running

To run the project, you need to have Go and Docker installed.

1.  **Start the services:**

    ```bash
    docker compose up -d
    ```

2.  **Run the application:**

    ```bash
    go run cmd/main.go
    ```

*   **Create a note:**

    ```bash
    curl --location --request POST 'localhost:8080/create' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "title":"Something interesting",
        "content": "Lorem ipsum..."
    }'
    ```

*   **Get a note:**

    ```bash
    curl --location --request GET 'localhost:8080/get?note_id=<your_note_id>'
    ```

# Development Conventions

*   **Tracing:** The project uses OpenTelemetry for tracing. Traces are sent to a Jaeger instance, which is configured in `cmd/main.go`.
*   **Storage:** The `storage` package encapsulates the logic for interacting with the Redis database.
*   **API:** The `server` package defines the HTTP handlers and API endpoints.
*   **Models:** The `models` package defines the data structures used in the application.
