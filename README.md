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
