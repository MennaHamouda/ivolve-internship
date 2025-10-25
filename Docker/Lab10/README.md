# Docker Lab10 / Docker-1

Small README for the project in this folder.

## What this repo contains
- A simple Java application (Spring Boot starter application) under `src/main/java/com/example/demo`.
- `pom.xml` (Maven build file).
- `Dockerfile` to build a container image.

## Prerequisites
- Java JDK (11+ recommended) installed and on PATH.
- Maven (mvn) installed for building the project.
- Docker installed and running to build/run images.

> Note: I assume the application uses the default Spring Boot port 8080. If your application configures a different port, change the examples below accordingly.

## Build with Maven
From the project root (where `pom.xml` is):

```bash
# Build the application (skip tests for a quicker build)
mvn clean package -DskipTests
```

After a successful build the runnable jar will be in `target/` (e.g. `target/*.jar`).

## Run the app locally (jar)

```bash
# Run the built jar
java -jar target/*.jar
```

The app should start on port 8080 by default.

## Build and run with Docker
From the project root (where `Dockerfile` is):

```bash
# Build the Docker image (tag it appropriately)
docker build -t docker-1:latest .

# Run the container, mapping port 8080
docker run --rm -p 8080:8080 docker-1:latest
```

Adjust `-p hostPort:containerPort` if your app uses a different port.

## Useful Maven Docker alternative (optional)
If you prefer a container image built by Spring Boot plugin (requires plugin configured in `pom.xml`):

```bash
mvn spring-boot:build-image -DskipTests
```

## Troubleshooting
- "Port already in use": stop the process using that port or change mapping (e.g. `-p 9090:8080`).
- Docker permission errors: ensure your user can run Docker or use `sudo` where required.
- Build failures: run `mvn clean package` and inspect the output; fix compile/test errors if any.

## Files changed / created
- `README.md` — added (this file).

## Assumptions
- App uses Spring Boot and listens on 8080.
- Standard Maven project layout.

If you'd like, I can:
- Add more details (project description, endpoints) by reading the source.
- Add a Docker image name/tag and CI examples.

