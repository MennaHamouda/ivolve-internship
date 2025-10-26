# Lab 13: Custom Docker Network for Microservices

## 📌 Objective
Implement a microservices architecture with custom Docker networking by running multiple frontend containers and a backend container, verifying which can communicate based on their networks.

---

## ✅ Steps

### 1️⃣ Clone the Repository
\`\`\`bash
git clone https://github.com/Ibrahim-Adel15/Docker5.git
cd Docker5
\`\`\`

---

### 2️⃣ Frontend Dockerfile
Create inside \`frontend/\`:
\`\`\`Dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
\`\`\`

Build the image:
\`\`\`bash
cd frontend
docker build -t frontend-app .
cd ..
\`\`\`

---

### 3️⃣ Backend Dockerfile
Create inside \`backend/\`:
\`\`\`Dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY . /app
RUN pip install flask
EXPOSE 5000
CMD ["python", "app.py"]
\`\`\`

Build the image:
\`\`\`bash
cd backend
docker build -t backend-app .
cd ..
\`\`\`

---

### 4️⃣ Create Custom Network
\`\`\`bash
docker network create ivolve-network
\`\`\`

---

### 5️⃣ Run Containers

#### ✅ Backend (custom network)
\`\`\`bash
docker run -d --name backend --network ivolve-network -p 5001:5000 backend-app
\`\`\`

#### ✅ Frontend1 (custom network)
\`\`\```bash
docker run -d --name frontend1 --network ivolve-network -p 5002:5000 frontend-app
\`\`\`

#### ✅ Frontend2 (default network)
\`\`\`bash
docker run -d --name frontend2 -p 5003:5000 frontend-app
\`\`\`

---

## 🔍 Verify Communication

✅ Works — Same network:
\`\`\`bash
docker exec -it frontend1 curl http://backend:5000
\`\`\`

❌ Fails — Different network:
\`\`\```bash
docker exec -it frontend2 curl http://backend:5000
\`\`\`

---

## 🧹 Cleanup
\`\`\`bash
docker rm -f frontend1 frontend2 backend
docker network rm ivolve-network
\`\`\`

---

## ✅ Summary

| Service | Network | Can reach backend? |
|---------|---------|------------------|
| frontend1 | ivolve-network | ✅ Yes |
| frontend2 | default network | ❌ No |

---
🎯 **Microservices must share a Docker network to communicate**