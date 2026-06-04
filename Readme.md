
# 🚀 Travel Website CI/CD Pipeline (DevOps Project)

## 📌 Project Overview

This project demonstrates a complete **CI/CD pipeline** for a static travel website using modern DevOps tools. The pipeline automates code integration, quality analysis, containerization, image scanning, and deployment.

---

# 🏗️ Architecture Flow

```text
GitHub → Jenkins → SonarQube → Trivy Scan → Docker Build → DockerHub → Deploy → Health Check
```

---

# ⚙️ Tech Stack

* Git & GitHub
* Jenkins (CI/CD Automation)
* Docker
* DockerHub
* SonarQube (Code Quality Analysis)
* Linux (Ubuntu EC2)

---

# 🔄 CI/CD Pipeline Steps

## 1️⃣ GitHub (Source Code Management)

* Developer pushes code to GitHub repository
* Jenkins webhook automatically triggers pipeline

📸 Screenshot:

![GitHub Repository](Screenshots/github-repo.png)

---

## 2️⃣ Jenkins (Automation Server)

* Jenkins pulls latest code from GitHub
* Executes defined pipeline stages

📸 Screenshot:

![Jenkins Pipeline](Screenshots/jenkins.png)

---

## 3️⃣ SonarQube Analysis

* Performs static code analysis
* Detects:

  * Bugs
  * Code Smells
  * Security Vulnerabilities

📌 Quality Gate ensures only clean code proceeds.

📸 Screenshot:

![SonarQube Analysis](Screenshots/sonarqube.png)

---

## 4️⃣ Trivy Security Scan

* Scans Docker images for vulnerabilities
* Detects security issues before deployment



## 5️⃣ Docker Image Build

* Application is containerized using Docker
* Image is tagged with build number

```bash
docker build -t travel-website:${BUILD_NUMBER} .
```

📸 Screenshot:

![Docker Build](Screenshots/docker-build.png)

---

## 6️⃣ DockerHub Push

* Docker image pushed to DockerHub registry
* Versioning handled using build number and latest tag

📸 Screenshot:

![DockerHub Push](Screenshots/docker-hub.png)

---

## 7️⃣ Deployment Stage

* Container deployed on EC2 instance
* Application exposed on port `8081`

```bash
docker run -d -p 8081:80 travel-website
```

📸 Screenshot:

![Docker Deployment](Screenshots/docker_ps.png)

---

## 8️⃣ Health Check

* Verifies application is running
* Uses curl to test endpoint

```bash
curl http://localhost:8081
```

📸 Screenshot:

![Application Output](Screenshots/localhost.png)

---

# 🔐 Jenkins Credentials Used

| Credential ID   | Purpose                  |
| --------------- | ------------------------ |
| dockerhub-creds | DockerHub Login          |
| sonar-token     | SonarQube Authentication |

---

# 📊 Pipeline Features

* ✔ Fully Automated CI/CD
* ✔ Code Quality Check (SonarQube)
* ✔ Security Scanning (Trivy)
* ✔ Docker Containerization
* ✔ DockerHub Integration
* ✔ Auto Deployment
* ✔ Health Verification

---

# 🚀 How to Run Project

```bash
# Clone Repository
git clone https://github.com/your-repo.git

# Open Jenkins
http://<EC2-IP>:8080

# Run Pipeline
Click "Build Now"
```

---

# 📸 Final Output

```text
Application runs on:
http://<EC2-IP>:8081
```

---

# 🏆 Key Learning Outcomes

* CI/CD Pipeline Automation
* Jenkins Pipeline Scripting
* Docker Container Lifecycle Management
* DevSecOps Basics (Trivy Integration)
* Code Quality Enforcement using SonarQube
* Real-world Deployment Workflow

