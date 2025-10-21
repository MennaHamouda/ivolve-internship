# 🧩 Lab 6: Building and Packaging Java Applications with Gradle

## 📘 Objective  
This lab demonstrates how to build and package a Java application using **Gradle**.  
You’ll learn how to install Gradle, run unit tests, build the application, and verify that it runs successfully.

---

## 🛠️ Steps

### 1. Install Gradle  
Make sure Gradle is installed on your system:  
```bash
sudo apt update
sudo apt install gradle -y
```
Verify the installation:  
```bash
gradle -v
```

---

### 2. Clone the Source Code  
Clone the project from the GitHub repository:  
```bash
git clone https://github.com/Ibrahim-Adel15/build1.git
cd build1
```

---

### 3. Run Unit Tests  
Execute the unit tests to ensure everything works properly:  
```bash
gradle test
```

---

### 4. Build the Application  
Build and package the Java application into a JAR file:  
```bash
gradle build
```

✅ The build artifact will be generated at:
```
build/libs/ivolve-app.jar
```

---

### 5. Run the Application  
Run the generated JAR file:  
```bash
java -jar build/libs/ivolve-app.jar
```

---

### 6. Verify the Application  
If the setup is successful, you should see the application output in the terminal confirming it’s running correctly.

---

## 📂 Project Structure  
```
build1/
 ├── src/
 │   ├── main/
 │   └── test/
 ├── build.gradle
 ├── settings.gradle
 └── README.md
```

---

## 🧠 Notes  
- Ensure Java (JDK 17 or later) is installed before running Gradle.  
- Run `gradle clean` before rebuilding if you modify the source code.
