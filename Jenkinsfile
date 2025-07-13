pipeline {
    agent any
    environment {
        // Define the SonarQube scanner tool name configured in Jenkins
        SONAR_HOME = tool "Sonar"
    }
    stages {
        stage("Clone code from GitHub") {
            steps {
                // Pull the source code from the specified GitHub repository and branch
                git url: "https://github.com/krishnaacharyaa/wanderlust.git", branch: "devops"
            }
        }
        stage("SonarQube Quality Analysis") {
            steps {
                // Analyze the code quality using SonarQube scanner
                withSonarQubeEnv("Sonar") {
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=devsecops_ci_cd -Dsonar.projectKey=DevSecOps"
                }
            }
        }
        stage("OWASP Dependency Check") {
            steps {
                // Scan dependencies for known vulnerabilities
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'OWASP'
                // Publish the XML report for review
                dependencyCheckPublisher pattern: '/dependency-check-report.xml'
            }
        }
        stage("Sonar Quality Gate Scan") {
            steps {
                // Wait for SonarQube to return the quality gate status
                timeout(time: 2, unit: "MINUTES") {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File System Scan") {
            steps {
                // Scan the file system and generate an HTML report for vulnerabilities and secrets
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("Deploy using Docker Compose") {
            steps {
                // Deploy the application using Docker Compose
                sh "docker-compose up -d"
            }
        }
    }
}
