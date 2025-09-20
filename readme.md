# Original Habr article:

[Трейсинг в Go — это просто](https://habr.com/ru/post/710644/)

# Notes service

```bash
docker compose up -d 
go run cmd/main.go
```

Create note:
```bash
curl --location --request POST 'localhost:8080/create' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title":"Something interesting",
    "content": "Lorem ipsum..."
}'
```

Get note: 

```bash
curl --location --request GET 'localhost:8080/get?note_id=7411ff79-fd1d-46ab-b9f8-21105cd770ce'
```

## Task runner via mise

Install the [Task](https://taskfile.dev) CLI with [mise](https://mise.jdx.dev) to use the repository automation defined in `Taskfile.yml`:

```bash
mise plugins install task https://github.com/mise-plugins/task
mise use -g task@latest
task --version
```

Run project commands with Task (Docker services start outside the container; the Go app runs locally):

```bash
task deps   # download Go modules
task up     # start Redis and Jaeger via docker compose
task run    # run infra + go run ./cmd
task down   # stop supporting containers
```
