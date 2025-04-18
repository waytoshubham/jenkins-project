# 🚀 Jenkins + Maven CI/CD Setup using Docker

This guide walks you through setting up Jenkins in Docker with Maven support and running a Maven build on a Java project.

---

## 📁 Project Structure

Make sure your project folder looks like this:

```
jenkins-project/ └── hello-java-maven/ ├── src/ └── pom.xml
```

---

### 🐳 Step 1: Run Jenkins in Docker

Jenkins official image does **not** include Maven by default. We'll use the `jenkins/jenkins:lts-jdk11` image, then manually install Maven inside the container.

```
docker run -d -p 8080:8080 -p 50000:50000 ^
  -v "D:/Internship Project/jenkins-project/hello-java-maven":/var/jenkins_home/workspace ^
  --name jenkins-ci jenkins/jenkins:lts-jdk11
```

---

### 🔑 Step 2: Get Jenkins Admin Password

To unlock Jenkins on first launch:

```
docker exec -it jenkins-ci cat /var/jenkins_home/secrets/initialAdminPassword
```

Paste the password into the Jenkins UI **(http://localhost:8080)**

---

### 🔧 Step 3: Install Maven in Jenkins Container

Enter the Jenkins container as root:

```
docker exec -u 0 -it jenkins-ci bash
```

Install Maven:

```
apt-get update
apt-get install -y maven
```

Check Maven installation:

```
mvn -v
```

---

### 🧰 Step 4: Configure Maven in Jenkins UI

    - Go to Jenkins UI **(http://localhost:8080)** → Global Tool Configuration

    - Under Maven, click Add Maven

    - Name it: Maven3

    - Tick ✅ Install automatically and select the latest version

    - Click Save

---

### 📦 Step 5: Create a Jenkins Job

        - From Jenkins Dashboard → New Item

        - Enter a name: hello-java-maven

        - Choose Freestyle project → Click OK

        - In Build Configuration:

        - Go to Build section

        - Click Add build step → Invoke top-level Maven targets

        - Goals: clean package

        - POM: pom.xml (no path needed if it's in root of workspace)

        - Save the job

---

### 🏃 Step 6: Run the Build

    - Go to Jenkins Dashboard → hello-java-maven → Build Now
    - Click Build Now. The console output should show Maven running and the build status.
