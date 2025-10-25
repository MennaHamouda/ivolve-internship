# Stage 1: Build Stage
FROM maven AS builder

WORKDIR /app

COPY . .

RUN mvn  package 

#  Stage 2: Runtime Stage 
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copy JAR file from builder stage
COPY --from=builder /app/target/demo-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
