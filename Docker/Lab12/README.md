# Lab 12: Docker Volume and Bind Mount with Nginx

## 🎯 Objective
- Persist Nginx logs using Docker Volume.
- Serve custom HTML from the host machine using Bind Mount.
- Validate updates and logs persistence.

---

## ✅ Steps

### 1️⃣ Create Docker Volume
```bash
docker volume create nginx_logs
docker volume ls
docker volume inspect nginx_logs
```

### 2️⃣ Create Directory Structure for Bind Mount
```bash
mkdir -p nginx-bind/html
```

### 3️⃣ Create Custom HTML File
```bash
echo "Hello from Bind Mount" > nginx-bind/html/index.html
```

### 4️⃣ Run Nginx Container
```bash
docker run -d --name nginx-lab12 \
  -p 8080:80 \
  -v nginx_logs:/var/log/nginx \
  -v $(pwd)/nginx-bind/html:/usr/share/nginx/html \
  nginx
```

### 5️⃣ Validate Served Page
```bash
curl http://localhost:8080
```

Expected:
```
Hello from Bind Mount
```

### 6️⃣ Update index.html & Validate Live Change
```bash
echo "Updated content from Bind Mount" > nginx-bind/html/index.html
curl http://localhost:8080
```

### 7️⃣ Verify Logs Are Persisted
```bash
docker exec -it nginx-lab12 ls /var/log/nginx
```

### 8️⃣ Cleanup
```bash
docker rm -f nginx-lab12
docker volume rm nginx_logs
```

---

## 📝 Summary

| Feature        | Uses | Purpose |
|----------------|------|---------|
| Volume         | `/var/log/nginx` | Persist logs independent of container lifecycle |
| Bind Mount     | `/usr/share/nginx/html` | Serve and update content directly from host |

✅ This lab demonstrates the difference between data persistence (volume) and real-time sync (bind mount).

---

