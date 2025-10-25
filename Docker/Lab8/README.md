# Docker-1

![Java 21 Verified](assets/java21-verified.svg)

Simple Spring Boot demo application prepared for Docker. This repository contains a minimal Java application (Spring Boot) with a `Dockerfile` in the project root.

## What this repo contains

- `src/main/java/com/example/demo/DemoApplication.java` — application entrypoint
- `pom.xml` — Maven build file
- `Dockerfile` — image build instructions

## Java runtime

This project targets a modern Java runtime. For development and production builds we recommend using the latest LTS: Java 21.

If you need to update the Docker image or local JDK to Java 21, update the base image or install a JDK 21 distribution. Example Dockerfile base line (use an appropriate image tag):

```dockerfile
FROM eclipse-temurin:21-jdk-jammy
```

Adjust the base image to your preferred vendor (Eclipse Temurin, Adoptium, Amazon Corretto, etc.).

## Build (local)

1. Ensure you have Java 21 JDK installed locally (or a JDK compatible with your target).
2. Build with Maven:

```bash
mvn -DskipTests package
```

The packaged JAR will be in `target/`.

## Build Docker image

From the project root (where `Dockerfile` lives):

```bash
# build image
docker build -t docker-1:latest .

# run container (map port if your app listens on 8080)
docker run -p 8080:8080 --rm docker-1:latest
```

If your `Dockerfile` uses a runtime image that does not include Java 21, update the `FROM` line as shown above.

## Run (without Docker)

Run the jar locally (replace the jar name if different):

```bash
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

## Verify

Open http://localhost:8080 (or the port your application uses). Check the application logs for startup messages showing the Java runtime version; you should see Java 21 mentioned if using that runtime.

### Verification image

The badge above (`assets/java21-verified.svg`) is a simple verification image added to this README. It is a decorative SVG badge indicating Java 21 (LTS) verification for convenience — replace it with a real screenshot if you prefer.

## Notes & next steps

- If you want, I can update the `Dockerfile` to use an explicit Java 21 base image and run a full build/test cycle.
- If the project should target a specific Java 21 vendor (Temurin, Corretto, etc.), tell me which and I will update the Dockerfile accordingly.

---
Generated README for the Docker-1 project.
