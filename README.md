# DevSecOps End-to-End CI/CD Pipeline with Jenkins, SonarQube, OWASP, and Trivy

## 📌 Project Overview
This project showcases a complete **DevSecOps CI/CD pipeline** that performs automated code quality and security checks before deploying an application. 

### Tools Used
- **Jenkins**: Automation server to orchestrate the CI/CD pipeline
- **SonarQube**: Static code analysis tool
- **OWASP Dependency-Check**: Detects known vulnerabilities in project dependencies
- **Trivy**: Scans filesystem and project for misconfigurations and vulnerabilities
- **Docker + Docker Compose**: For application deployment

---

## 🔄 Workflow Summary

1. **Code Cloning**
   - Jenkins pulls source code from GitHub:  
     `https://github.com/krishnaacharyaa/wanderlust.git` (branch: `devops`)

2. **Static Code Analysis (SonarQube)**
   - Jenkins sends the code to SonarQube for analysis
   - Identifies bugs, code smells, vulnerabilities
   - View results on SonarQube at port `9000`

3. **Dependency Vulnerability Scan (OWASP Dependency-Check)**
   - Scans third-party libraries for known CVEs
   - Report: `dependency-check-report.xml`

4. **Quality Gate Check (SonarQube)**
   - Jenkins waits for quality gate status
   - If passed, continues the pipeline

5. **File System Vulnerability Scan (Trivy)**
   - Scans project directory for secrets, misconfigurations
   - Report: `trivy-fs-report.html`

6. **Deployment (Docker Compose)**
   - App is deployed only if pipeline passes all scans
   - Accessible at port `5173`

---

## ☁️ Infrastructure Setup

### 1. EC2 Instance (Ubuntu)
- Initially used **t2.medium** but upgraded to **t2.large** due to OWASP Dependency-Check memory usage
- Storage: **10 GB**

### 2. Docker and Docker Compose
```bash
sudo apt update -y
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker ubuntu
reboot
```
### 3. Jenkins Setup (Port 8080)
```bash
sudo apt update -y
sudo apt install fontconfig openjdk-17-jre -y

wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update -y
sudo apt install jenkins -y
```
Access Jenkins: http://<EC2_IP>:8080
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
### 3. SonarQube Setup (Port 9000)
🔧 SonarQube Setup (Port 9000)
```bash
docker run -dit --name sonarqube -p 9000:9000 sonarqube:lts-community
```
-Create a token in SonarQube
-Create a webhook named jenkins
-Add SonarQube server and credentials in Jenkins
-Install tool: SonarQube Scanner via Jenkins > Manage Jenkins > Global Tool Configuration

### 4.Install Trivy
```bash
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null

sudo apt update -y
sudo apt install trivy -y
```
## Jenkins Plugin Requirements
   -SonarQube Scanner
   -Sonar Quality Gates
   -OWASP Dependency-Check
   -Docker

## ✅ Grant Docker Access to Jenkins
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
```

## 📂 Reports Location
   OWASP Report: dependency-check-report.xml
   → Use an XML viewer or publish in Jenkins build output
   
   Trivy Report: trivy-fs-report.html
   → Open in browser or view using:
   ```bash
   cat trivy-fs-report.html
   ```

## 🧪 Jenkins Pipeline
The pipeline used in this project is located in a separate file for clarity.
Please refer to the Jenkinsfile for full pipeline implementation with explanatory comments.

## 🙌 What I Learned
   -Set up a real-world DevSecOps pipeline from scratch
   -Gained hands-on experience with SonarQube, OWASP, Trivy, and Docker
   -Learned how to install and configure Jenkins and integrate external tools
   -Resolved Docker permission issues for Jenkins
   -Optimized EC2 instance type for memory-heavy operations
   -Understood the importance of security and quality gates in modern CI/CD pipelines




