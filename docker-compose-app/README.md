# Docker Compose application

This project contains the practical implementation for homework 08:

- Flask/Gunicorn web service;
- PostgreSQL database service;
- persistent named volume;
- database healthcheck;
- multi-stage Dockerfile;
- GitHub Actions build, test and GHCR publication.

## Local run

```bash
docker compose config --quiet
docker compose up --detach --build --wait
docker compose ps
curl --fail http://localhost:8080/health
curl --fail http://localhost:8080/
```

The `/` endpoint stores a visit counter in PostgreSQL. Data remains available
when containers are recreated because PostgreSQL uses a named volume.

Stop containers without deleting data:

```bash
docker compose down
```

Stop containers and delete test data:

```bash
docker compose down --volumes --remove-orphans
```

## Environment variables

Defaults are provided only for local development. To use custom values:

```bash
cp .env.example .env
```

Edit `.env` locally. The file is excluded from the Docker build context and
must not be committed with a real password.

## Published image

Pushes to `main` build and publish:

```text
ghcr.io/antonzhdanko/docker-compose-homework:<commit-sha>
ghcr.io/antonzhdanko/docker-compose-homework:latest
```

Pull requests build and test the image without publishing it.

## Slack notification

Create a repository secret named `SLACK_WEBHOOK`. The workflow sends the job
status, image name and commit tag to that webhook. If the secret is not yet
configured, the workflow reports that notification was skipped without
exposing or inventing a token.
