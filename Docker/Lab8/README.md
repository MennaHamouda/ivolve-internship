docker build -t docker-1:latest .
docker run -p 8080:8080 --rm docker-1:latest
# Demo Spring Boot — Docker Lab 8

![Java 21 Verified](assets/java21-verified.svg)

This repository contains a minimal Spring Boot demo application prepared for Docker (Lab 8).

Key files
- `src/main/java/com/example/demo/DemoApplication.java` — application entrypoint
- `pom.xml` — Maven build file
- `Dockerfile` — image build instructions

## Table of contents
- [Prerequisites](#prerequisites)
- [Build (Maven)](#build-maven)
- [Run locally](#run-locally)
- [Build & run with Docker](#build--run-with-docker)
- [Project structure](#project-structure)
- [Troubleshooting & notes](#troubleshooting--notes)
- [Next steps](#next-steps)

## Prerequisites
- Java 21 JDK (recommended) or a compatible JDK
- Maven 3.6+ (for building)
- Docker (optional, for container image build/run)

If you don't have Java 21 locally you can still build inside Docker or use an SDK manager (sdkman/apt/homebrew) to install the required JDK.

## Build (Maven)
From the project root run:

```bash
mvn -DskipTests package
```

The built artifact will be placed in `target/` (look for `target/*.jar`).

## Run locally
Run the packaged JAR:

```bash
java -jar target/*.jar
```

By default the application listens on port 8080. Open http://localhost:8080 to verify.

## Build & run with Docker
This project contains a `Dockerfile` at the repository root. Use the following commands from the project root to build and run the container:

```bash
# Build the image (tags the image as ilvolve/demo:lab8)
docker build -t ilvolve/demo:lab8 .

# Run the container and map port 8080
docker run --rm -p 8080:8080 ilvolve/demo:lab8
```

Notes:
- The `Dockerfile` in this repo is intended to produce a small runtime image. If you want an explicit Java 21 base image, update the `FROM` line to a Java 21 vendor image such as `eclipse-temurin:21-jdk-jammy`.
- If the application binds a different port, adjust `-p HOST:CONTAINER` accordingly.

## Project structure

```
.
├── Dockerfile
├── pom.xml
├── src/
│   └── main/java/com/example/demo/DemoApplication.java
└── target/  (build output)
```

## Troubleshooting & tips
- If `mvn package` fails, run `mvn -X package` to get debug output and ensure your JAVA_HOME points to a compatible JDK.
- If Docker build fails due to base image/version mismatch, change the base image tag in `Dockerfile` to a supported Java image.

## Next steps (suggestions)
- Add a small health-check endpoint and a README section with example API calls (curl).
- Add CI (GitHub Actions) that builds the Maven project and optionally builds/pushes the Docker image.

## License
This project doesn't include a license file. Add one (for example `LICENSE` with MIT) if you intend to publish or reuse this code.

---
Created/updated README for Docker Lab 8 Spring Boot demo.
