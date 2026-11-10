# Flask Kubernetes Local CI/CD Demo

This project contains a minimal Flask application that is packaged in Docker and deployed to a local Kubernetes cluster.

## What the app does

The Flask app exposes a single endpoint at `/` and returns:

`Hello from Flask inside Kubernetes!`

## Local development

Run the app in Docker Compose:

```bash
docker-compose up --build
```

Then open:

```text
http://localhost:5000/
```

## Docker build

```bash
docker build -t flask-k8s-app .
```

## Kubernetes deploy

If you already have a local cluster available (such as Docker Desktop Kubernetes), deploy using:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Then the app should be reachable on:

```text
http://localhost:30007/
```

## Local CI/CD workflow

This repository includes a GitHub Actions workflow at `.github/workflows/local-cicd.yml`.

It will:

1. Check out the code
2. Install Python dependencies
3. Validate the Python app with `compileall`
4. Build a Docker image
5. Push it to Docker Hub using secrets
6. Apply the Kubernetes manifests to the local cluster
7. Wait for rollout completion

### Required GitHub secrets

Set these in the repository settings:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

The workflow uses a self-hosted runner and assumes a local Kubernetes context such as Docker Desktop Kubernetes is available on that machine.
