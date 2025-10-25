# Use Maven with Java 17 (build + run)
FROM maven

# Set working directory
WORKDIR /app

# Copy all project files
COPY . .

# Build Spring Boot application
RUN mvn package 

# Expose App Port
EXPOSE 8080

# Run the generated jar file
CMD ["java", "-jar", "target/demo-0.0.1-SNAPSHOT.jar"]
