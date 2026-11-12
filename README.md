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
5. Push it to GitHub Container Registry (GHCR) using the workflow's `GITHUB_TOKEN`
6. Apply the Kubernetes manifests to the local cluster
7. Wait for rollout completion

### GHCR and local cluster setup

The workflow needs `contents: read` and `packages: write` permissions, which are declared in the workflow. No Docker Hub credentials are needed.

GHCR packages are private by default. For the local Kubernetes cluster to pull the image, either make the `flask-k8s-app` package public in GitHub's package settings, or configure a Kubernetes `imagePullSecret` with a GitHub token that has `read:packages` access.

The workflow uses a self-hosted runner and assumes a local Kubernetes context such as Docker Desktop Kubernetes is available on that machine.
