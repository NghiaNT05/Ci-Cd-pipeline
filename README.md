# CI/CD Java Pipeline Project

## 📌 Project Overview

This is a demo Java project that demonstrates a complete CI/CD pipeline using:

- **Jenkins** for automation
- **Maven** for build and test
- **SonarQube** for code quality analysis
- **Trivy** for container vulnerability scanning
- **Docker** for containerization
- **Remote triggering** of a CD pipeline after successful CI

---


## 🚀 CI/CD Pipeline Stages (Jenkinsfile)

The `Jenkinsfile` defines the following stages:

### 🧹 1. Clean Workspace
Cleans the Jenkins workspace before building.

### 🧬 2. Checkout from Git
Pulls the latest code from the `main` branch of GitHub.

### 🛠 3. Build Application
Builds the project using Maven:
```bash
mvn clean package

✅ 4. Run Tests

Executes all unit tests:

mvn test

📊 5. SonarQube Analysis

Performs static code analysis with SonarQube.
🛑 6. Quality Gate Check

Waits for SonarQube to pass the quality gate. The pipeline will not abort if the gate fails (this is configurable).
🔍 7. Trivy Security Scan

Scans the Docker image for high and critical vulnerabilities using Trivy

:

trivy image your-image-name

🐳 8. Build & Push Docker Image

    Builds the Docker image.

    Pushes the image to Docker Hub with a unique tag and as latest.

Docker image tag format:

<dockerhub-username>/<app-name>:<release>-<build-number>

🧽 9. Cleanup Artifacts

Removes local Docker images to save space on the Jenkins agent.
🚀 10. Trigger CD Pipeline

Uses a remote Jenkins API token to trigger a downstream deployment pipeline (GitOps-style).
🧰 Tools Used
Tool	Purpose
Jenkins	Orchestration of CI/CD steps
Maven	Build and test automation
SonarQube	Code quality and coverage checks
Docker	Build and run application in containers
Trivy	Container security scanning
GitHub	Source code repository
