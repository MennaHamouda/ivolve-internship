# Demo Application (Docker)

This repository contains a simple Spring Boot demo application and a `Dockerfile` that copies an already-built JAR into the image and runs it with OpenJDK.

Summary
- The Dockerfile copies `target/demo-0.0.1-SNAPSHOT.jar` to the image as `app.jar` and runs it with `java -jar app.jar`.
- The container exposes port `8080`.

Quick contract
- Input: A built JAR at `target/demo-0.0.1-SNAPSHOT.jar` (produced by Maven `package`).
- Output: A Docker image that runs the JAR and serves on port 8080.

Build the application (locally)
1. Make sure Java and Maven are installed.
2. From the project root run:

```bash
mvn clean package -DskipTests
```

That will produce the JAR under `target/demo-0.0.1-SNAPSHOT.jar`.

Build the Docker image

```bash
docker build -t demo-app:latest .
```

Note: The `Dockerfile` expects the JAR at `target/demo-0.0.1-SNAPSHOT.jar`. If your build produces a different filename, either update the `Dockerfile` or rename the JAR before building the image.

Run the container

```bash
# run and map container port 8080 to localhost:8080
docker run --rm -p 8080:8080 demo-app:latest
```

Verify the app

```bash
# after the container starts, try:
curl http://localhost:8080/
```

Troubleshooting & notes
- If Docker build fails because the JAR is missing, ensure you ran `mvn package` and that the artifact name matches `target/demo-0.0.1-SNAPSHOT.jar`.
- The image uses `openjdk:17-jdk-slim`. If your app requires a different Java version, update the `Dockerfile` base image accordingly.
- To speed up development, you can run the app locally without Docker using:

```bash
mvn spring-boot:run
```

If you want, I can:
- Add a `docker-compose.yml` for easy local runs.
- Modify the `Dockerfile` to build the JAR inside the image (multi-stage build).
- Add a small healthcheck or readiness probe in the Dockerfile.

