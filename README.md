# Flask CI/CD Pipeline

A simple Flask API with a fully automated CI/CD pipeline using GitHub Actions. Part of a hands-on DevOps learning path — this is **Tier 3: CI/CD**.

## What It Does

On every push or pull request to `master`, GitHub Actions automatically:

1. **Runs tests** — installs dependencies and runs pytest
2. **Builds the Docker image** — only if tests pass
3. **Verifies the container** — starts it up and hits the `/health` endpoint

No manual steps. Push code, get a green checkmark (or a red X if something broke).

## The App

A minimal Flask API with two routes:

| Route | Response |
|-------|----------|
| `GET /` | `{"message": "Welcome to Ben's Tier 3 DevOps project"}` |
| `GET /health` | `{"status": "healthy"}` |

## Pipeline Architecture

```
git push
   │
   ▼
┌─────────────────────┐
│  Job 1: Run Tests   │
│  - Setup Python     │
│  - Install deps     │
│  - pytest -v        │
└─────────┬───────────┘
          │ (pass)
          ▼
┌─────────────────────┐
│  Job 2: Docker      │
│  - Build image      │
│  - Run container    │
│  - curl /health     │
└─────────────────────┘
          │
          ▼
      ✅ or ❌
```

## Run Locally

```bash
# Install dependencies
pip install -r requirements.txt

# Run tests
pytest -v

# Run the app
python app.py
# Visit http://localhost:5000

# Or run with Docker
docker build -t flask-ci-pipeline .
docker run -p 5000:5000 flask-ci-pipeline
```

## Project Structure

```
.
├── app.py                         # Flask API
├── test_app.py                    # Pytest tests
├── requirements.txt               # Python dependencies
├── Dockerfile                     # Container definition
└── .github/workflows/ci.yml      # GitHub Actions pipeline
```

## Key Concepts Demonstrated

- **Continuous Integration** — tests run automatically on every push
- **Build verification** — Docker image is built and smoke-tested in the pipeline
- **Job dependencies** — Docker build only runs if tests pass first
- **Shift-left testing** — catch bugs at push time, not deploy time

## Part of a DevOps Learning Path

| Tier | Focus | Status |
|------|-------|--------|
| 1 | Containerization (Docker) | [Complete](https://github.com/Batkins44/jellyfin-docker) |
| 2 | Infrastructure as Code (Terraform + AWS) | [Complete](https://github.com/Batkins44/terraform-aws-webserver) |
| 3 | CI/CD (GitHub Actions) | **This project** |
| 4 | Configuration Management (Ansible) | Up next |
| 5 | Full Pipeline (all combined) | Planned |
