
## 📌 README — Lab 14: Node.js App with MySQL using Docker Compose

### 📍 Project Overview
This lab deploys a Node.js application connected to a MySQL database using Docker Compose.  
The application requires a database named **`ivolve`** to successfully initialize.

---

### ✅ Prerequisites
Before running the project, ensure the following are installed:

- Docker Engine
- Docker Compose v2+

Verify Docker Compose version:
```bash
docker compose version
```

---

### 📥 Clone the Source Code
```bash
git clone https://github.com/Ibrahim-Adel15/kubernets-app.git
cd kubernets-app
```

---

### 🧱 Dockerfile
The app is containerized using a Dockerfile located inside the project.

---

### 🐳 Docker Compose Configuration

Services included:
| Service | Description |
|---------|-------------|
| **app** | Node.js backend application |
| **db**  | MySQL database (stores persistent data using a volume) |

The app consumes database connection via environment variables:
- `DB_HOST`
- `DB_USER`
- `DB_PASSWORD`

A named volume `db_data` is used to persist MySQL data.

---

### 🚀 Run the Application
Build and start containers:
```bash
docker compose up -d --build
```

Check running containers:
```bash
docker ps
```

---

### 🔍 Testing & Verification

#### ✅ App running
Open browser:
```
http://localhost:3000
```

#### ✅ Health Check Endpoint
```bash
curl http://localhost:3000/health
```

#### ✅ Ready Check Endpoint
```bash
curl http://localhost:3000/ready
```

#### ✅ Verify Database
```bash
docker exec -it mysql-db mysql -u root -p
SHOW DATABASES;
```

You should see:
```
ivolve
```

#### ✅ Access Logs
Application logs are stored inside:
```
/app/logs/
```

If you mapped logs to host, check folder:
```bash
ls logs
```

---

### ☁ Push Docker Image to DockerHub

Tag your built app image:
```bash
docker tag kubernets-app <your-dockerhub-username>/kubernets-app:v1
```

Login:
```bash
docker login
```

Push:
```bash
docker push <your-dockerhub-username>/kubernets-app:v1
```

---

### 🧹 Stop & Cleanup
```bash
docker compose down
docker volume rm db_data
```

---

### ✅ Status Checklist

| Task | Status |
|------|:-----:|
| MySQL DB created | ✅ |
| Node.js app connected | ✅ |
| /health & /ready working | ✅ |
| Logs verified | ✅ |
| Image pushed to DockerHub | ✅ |
