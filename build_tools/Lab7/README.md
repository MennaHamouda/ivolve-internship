# Lab 7: Building and Packaging Java Applications with Maven

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/Ibrahim-Adel15/build2)  
[![Java Version](https://img.shields.io/badge/java-17-blue)](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)

## Objective
Build and package a Java application using Maven, run unit tests, and verify the application works correctly.

## Prerequisites
- Java JDK 17+
- Maven installed
- Git installed

## Steps

### 1. Clone the Repository
```bash
git clone https://github.com/Ibrahim-Adel15/build2.git
cd build2
```

### 2. Run Unit Tests
```bash
mvn test
```
- Executes all unit tests.
- Check console output for results.

### 3. Build the Application
```bash
mvn package
```
- Compiles the code, runs tests, and creates a JAR file.
- The artifact will be located at:
```
target/hello-ivolve-1.0-SNAPSHOT.jar
```

### 4. Run the Application
```bash
java -jar target/hello-ivolve-1.0-SNAPSHOT.jar
```

### 5. Verify Application
- Ensure the application runs without errors.
- Confirm the expected output in the console.

## Notes
- Ensure Maven and Java are properly configured in your system PATH.
- Maven automates compiling, testing, and packaging.
