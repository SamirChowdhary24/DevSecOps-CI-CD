# 💡 DevSecOps CI/CD Pipeline Project

A complete DevSecOps pipeline integrating **Jenkins**, **SonarQube**, **OWASP Dependency-Check**, **Trivy**, and **Docker Compose**. This project demonstrates how to securely build, analyze, scan, and deploy a full-stack application using open-source tools.

---

## 🔧 Tools Used

| Tool | Purpose |
|------|---------|
| **Jenkins** | CI/CD automation |
| **SonarQube** | Static code analysis |
| **OWASP Dependency-Check** | Detect vulnerable dependencies |
| **Trivy** | Vulnerability & secret scanning |
| **Docker + Docker Compose** | Containerization and orchestration |
| **GitHub** | Source code repository |

---

## 🛠️ Step-by-Step Implementation

### 1️⃣ Launch EC2 Instance

- Started with `t2.medium` (2 vCPU, 4 GB RAM), faced performance issues.
- Upgraded to `t2.large` (2 vCPU, 8 GB RAM) due to **OWASP scan memory needs**.
- Allocated **10 GB storage**.

### 2️⃣ Install Docker and Docker Compose

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker ubuntu
reboot
