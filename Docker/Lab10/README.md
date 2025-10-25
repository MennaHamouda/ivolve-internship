# Lab 10: Multi-Stage Docker Build for Node.js Application

## 📌 Objective
Containerize a Node.js application using a **multi-stage Docker build**, reducing the final image size by separating the build and runtime environments.

## 📥 Clone the Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1/App3
```

## 🐳 Dockerfile (Multi-Stage Build)

### ✅ Requirements
- Use **Maven base image** for the build stage
- Copy the source code into the container
- Build the application using `mvn package`
- Use a lightweight **Java base image** for the runtime stage
- Copy only the final JAR to keep image size minimal
- Expose port **8080**
- Run the JAR file

> (Place this Dockerfile inside the App3 folder)

```dockerfile
# ---------- Stage 1: Build ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app

COPY . .
RUN mvn clean package -DskipTests

# ---------- Stage 2: Runtime ----------
FROM eclipse-temurin:17-jre
WORKDIR /app

COPY --from=builder /app/target/demo-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```

## 🏗️ Build Docker Image
```bash
docker build -t app3-multi-stage .
```

✅ Compare image size with a single-stage build:
```bash
docker images | grep app3
```

## ▶️ Run the Container
```bash
docker run -d -p 8080:8080 --name app3-container app3-multi-stage
```

## ✅ Test the Application
Open browser or use curl:
```
http://localhost:8080
```

## ⛔ Stop and Remove Container
```bash
docker stop app3-container
docker rm app3-container
```

## ✅ Expected Output
- Application successfully builds and runs in a smaller Java runtime image
- Application responds on port **8080** when tested

## 📎 Notes
- Multi-stage builds significantly **reduce image size** by eliminating build dependencies from the final runtime image.
- Always verify the artifact name inside `target/` before copying it in the runtime stage.
