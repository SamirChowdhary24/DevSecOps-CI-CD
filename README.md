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
- Create a token in SonarQube
![WhatsApp Image 2025-07-08 at 22 26 48_c96e9561](https://github.com/user-attachments/assets/8712f51b-62cf-4c8c-b2a4-7029a9ab9480)

- Create a webhook named jenkins
![WhatsApp Image 2025-07-08 at 22 24 58_c1f2ad83](https://github.com/user-attachments/assets/26ac1d5d-83df-4034-b175-b8c6e75d27d4)

- Add SonarQube server and credentials in Jenkins
![WhatsApp Image 2025-07-11 at 12 15 34_44d13ade](https://github.com/user-attachments/assets/c2e8645e-f689-4f8c-bbd5-6e8b28d0023d)

- Install tool: SonarQube Scanner via Jenkins > Manage Jenkins > Global Tool Configuration
![WhatsApp Image 2025-07-11 at 12 22 38_d1b71039](https://github.com/user-attachments/assets/ba86de73-881a-41cd-a5bd-5d368c7cabdc)


### 4.Install Trivy
```bash
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null

sudo apt update -y
sudo apt install trivy -y
```
## Jenkins Plugin Requirements
![WhatsApp Image 2025-07-08 at 21 56 53_c4395b21](https://github.com/user-attachments/assets/707e66c8-2f6b-430c-b366-da270aaa88f0)
   - SonarQube Scanner
   - Sonar Quality Gates
   - OWASP Dependency-Check
   - Docker

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
# PIPELINE OVERVIEW
![WhatsApp Image 2025-07-12 at 22 16 36_5e933d8c](https://github.com/user-attachments/assets/f6d38a9e-0847-4820-a86e-bd3024cbd4c0)
# AWS INBOUND RULES
![WhatsApp Image 2025-07-12 at 22 19 19_efc471a1](https://github.com/user-attachments/assets/6ce75e02-6c24-4775-96fe-58046a00de61)
# APPLICATION DEPLOYED THROUGH DOCKER COMPOSE
![WhatsApp Image 2025-07-12 at 22 20 32_2835c4bb](https://github.com/user-attachments/assets/4e4474d6-9dbc-4523-bb0c-a7941cb40615)
# DEPENDENCY CHECK RESULTS
![WhatsApp Image 2025-07-12 at 22 29 38_c873e001](https://github.com/user-attachments/assets/375342a9-7a54-40d8-94d8-d75d58c29394)
# QUALITY GATES STATUS
![WhatsApp Image 2025-07-12 at 22 14 39_e1d6c75f](https://github.com/user-attachments/assets/5d70b94c-6c0c-4048-9f74-b0f98772eb39)
# TRIVY SCAN REPORT



## 🙌 What I Learned
   - Set up a real-world DevSecOps pipeline from scratch
   - Gained hands-on experience with SonarQube, OWASP, Trivy, and Docker
   - Learned how to install and configure Jenkins and integrate external tools
   - Resolved Docker permission issues for Jenkins
   - Optimized EC2 instance type for memory-heavy operations
   - Understood the importance of security and quality gates in modern CI/CD pipelines




