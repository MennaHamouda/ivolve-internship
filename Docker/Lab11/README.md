
# Lab 11: Managing Docker Environment Variables Across Build and Runtime

## ✅ Objective
Manage environment variables during Docker **build** and **runtime** for a Python Flask application.

---

## 🧩 Steps to Complete

### ✅ 1️⃣ Clone the Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-3.git
cd Docker-3
```

---

### ✅ 2️⃣ Create Dockerfile
```dockerfile
FROM python:3.10

# Environment variables for production (build-time)
ENV APP_MODE=production
ENV APP_REGION=canada-west

WORKDIR /app
COPY . /app

RUN pip install flask

EXPOSE 8080

CMD ["python", "app.py"]
```

---

### ✅ 3️⃣ Build Docker Image
```bash
docker build -t lab11-env-vars .
```

---

### ✅ 4️⃣ Run Container with Environment Variables in Command
(Using: development, us-east)
```bash
docker run -d -p 8080:8080   -e APP_MODE=development   -e APP_REGION=us-east   --name lab11-dev   lab11-env-vars
```

---

### ✅ 5️⃣ Set Environment Variables From File
Create **staging.env**:
```
APP_MODE=staging
APP_REGION=us-west
```

Run:
```bash
docker run -d -p 8081:8080   --env-file staging.env   --name lab11-staging   lab11-env-vars
```

---

### ✅ 6️⃣ Run Using Dockerfile Environment Variables
(Using: production, canada-west)
```bash
docker run -d -p 8082:8080   --name lab11-prod   lab11-env-vars
```

---

## 🔍 Verification (Optional)
Check variables:
```bash
docker exec -it lab11-dev printenv | grep APP_
```

---

## 🌍 Port Mapping Summary
| Environment | Mode | Region | URL |
|------------|------|--------|-----|
| Development | development | us-east | http://localhost:8080 |
| Staging | staging | us-west | http://localhost:8081 |
| Production | production | canada-west | http://localhost:8082 |

---

✅ Completed Successfully 🎯
