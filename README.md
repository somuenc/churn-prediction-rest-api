# churn-prediction-rest-api
Customer Churn Prediction - Flask API Backend + Streamlit Frontend (Dockerized)

## Running locally with Docker

This repository contains two services:
- `backend` (Flask + Gunicorn) listening on port **7860** inside the container
- `frontend` (Streamlit) listening on port **8501** inside the container

You can build images separately and run them together with Docker Compose (no proxy required).

1) Build images separately (optional — Compose can build for you):

```bash
# Build backend image
cd backend
docker build -t churn-backend:latest .

# Build frontend image
cd ../frontend
docker build -t churn-frontend:latest .
```

2) From the repository root start both services (use existing images):

```bash
docker compose up -d --no-build
```

To let Compose build images before starting (skip manual builds):

```bash
docker compose up -d --build
```

3) Stop and remove containers and network:

```bash
docker compose down
```

Notes:
- The frontend container reads `BACKEND_URL` from the environment. When using the provided `docker-compose.yml`, Compose sets it to `http://backend:7860` so the frontend can call the backend by service name.
- If you run containers manually instead of Compose, create and attach them to a common network so they can discover each other:

```bash
docker network create churn-net
docker run -d --name churn-backend --network churn-net -p 7860:7860 churn-backend:latest
docker run -d --name churn-frontend --network churn-net -p 8501:8501 -e BACKEND_URL=http://churn-backend:7860 churn-frontend:latest
```

Files:
- [docker-compose.yml](docker-compose.yml) (added to repo root)

