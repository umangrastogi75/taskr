# TASKR — Flask Task Manager

A minimal task manager built with Flask, ready for Docker and Kubernetes practice.

---

## Project Structure

```
taskapp/
├── app.py              # Flask application
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── templates/
│   └── index.html
└── k8s/
    ├── deployment.yaml
    ├── service.yaml
    └── pvc.yaml
```

---

## Run Locally

```bash
pip install -r requirements.txt
python app.py
# → http://localhost:5000
```

---

## Docker

### Build & run manually
```bash
docker build -t taskr .
docker run -p 5000:5000 -v taskr-data:/data taskr
```

### With docker-compose
```bash
docker-compose up --build
# → http://localhost:5000
```

---

## Kubernetes

### 1. Build and push image
```bash
docker build -t your-registry/taskr:latest .
docker push your-registry/taskr:latest
```
Update `image:` in `k8s/deployment.yaml` to match.

### 2. Apply manifests
```bash
kubectl apply -f k8s/pvc.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### 3. Check status
```bash
kubectl get pods
kubectl get svc taskr-service
```

### 4. Scale up/down
```bash
kubectl scale deployment taskr --replicas=4
```

---

## API Endpoints

| Method | Path              | Description        |
|--------|-------------------|--------------------|
| GET    | /                 | Web UI             |
| GET    | /api/tasks        | List all tasks     |
| POST   | /api/tasks        | Create a task      |
| PATCH  | /api/tasks/:id    | Toggle done/undone |
| DELETE | /api/tasks/:id    | Delete a task      |
| GET    | /health           | Health check       |
