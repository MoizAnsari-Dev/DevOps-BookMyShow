## 🎬 DevOps Project – BookMyShow Clone

This project is a BookMyShow-like ticket booking app built with a complete DevOps pipeline. It demonstrates:

📦 Containerization with Docker

⚙️ CI/CD automation using Jenkins

☸️ Kubernetes deployment on AWS EKS

🔍 Security scans (SonarQube, OWASP Dependency Check, Trivy)

📊 Monitoring with Prometheus + Grafana

📧 Email notifications for build status

## 📖 Table of Contents
- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Setup Instructions](#-setup-instructions)
  - [VM & AWS Setup](#1️⃣-vm--aws-setup)
  - [Install Jenkins](#2️⃣-install-jenkins)
  - [Install Docker](#3️⃣-install-docker)
  - [Install Trivy](#4️⃣-install-trivy)
  - [Setup SonarQube](#5️⃣-setup-sonarqube)
  - [Email Integration](#6️⃣-email-integration)
- [Jenkins Pipeline](#-jenkins-pipeline)
- [Kubernetes Deployment](#-kubernetes-deployment)
- [Monitoring with Prometheus & Grafana](#-monitoring-with-prometheus--grafana)
- [Future Improvements](#-future-improvements)
- [Contributing](#-contributing)
- [License](#-license)

## 🚀 Features  

### 🎟 Application Features  
- User-friendly ticket booking system (BookMyShow-like clone)  
- Runs on Node.js with simple setup  
- Exposed via Kubernetes Service (NodePort / LoadBalancer)  

### ⚙️ DevOps Features  
- 📦 **Containerization** → App is dockerized for portability  
- ⚙️ **CI/CD with Jenkins** → Automated pipeline with multiple stages  
- 🔍 **Code Quality & Security** →  
  - SonarQube Analysis  
  - OWASP Dependency Check  
  - Trivy vulnerability scanning  
- ☸️ **Kubernetes Deployment** → Deployed on AWS EKS cluster  
- 🐳 **DockerHub Integration** → Auto push images to DockerHub registry  
- 📧 **Email Notifications** → Jenkins sends build success/failure emails with logs  
- 📊 **Monitoring** → Prometheus + Grafana dashboards for app + Jenkins monitoring  



## 🏗 Architecture
```flowchart TD
  A[Developer] -->|Push Code| B[GitHub Repo]
  B -->|Webhook| C[Jenkins Pipeline]
  C --> D[SonarQube Scan]
  C --> E[OWASP & Trivy Scan]
  C --> F[Docker Build & Push]
  F --> G[AWS EKS Cluster]
  G --> H[Kubernetes Deployment & Service]
  H --> I[Users Access App]
  J[Prometheus & Grafana] --> C
  J --> G
```

## 🛠 Tech Stack  

### 🎟 Application  
- **Node.js / Express.js** → Backend service for BookMyShow clone  
- **NPM** → Package manager for dependencies  

### ⚙️ DevOps & CI/CD  
- **Git + GitHub** → Version control & repo hosting  
- **Jenkins** → CI/CD automation with multi-stage pipelines  
- **SonarQube** → Static code analysis & Quality gates  
- **OWASP Dependency-Check** → Dependency vulnerability scanning  
- **Trivy** → Container & filesystem vulnerability scanning  
- **Docker** → Containerization & image management  
- **DockerHub** → Container image registry  
- **Kubernetes (AWS EKS)** → Orchestration & deployment  

### 📊 Monitoring & Alerting  
- **Prometheus** → Metrics collection & monitoring  
- **Node Exporter** → Node-level metrics for Prometheus  
- **Grafana** → Dashboards & visualization  
- **Email Notifications** → Build success/failure alerts via Jenkins  

### ☁️ Cloud & Infrastructure  
- **AWS EC2** → Jenkins, Monitoring servers  
- **AWS IAM** → Access & security policies  
- **AWS EKS** → Managed Kubernetes cluster  

## 📂 Project Structure  

DevOps-BookMyShow/
│── bookmyshow-app/ # Node.js BookMyShow clone application
│ ├── package.json # Node.js dependencies
│ ├── server.js # Main server file (example entry point)
│ └── ... # Other app source files
│
│── Jenkinsfile # Jenkins CI/CD pipeline (build, scan, deploy)
│── deployment.yml # Kubernetes Deployment manifest
│── service.yml # Kubernetes Service manifest
│── BMS-Document.txt # Setup & installation guide (tools, Jenkins, AWS, K8s, monitoring)
│── README.md # Project documentation (this file)


## ⚡ Setup Instructions  

Follow these steps to set up the project from scratch:  

---

### 1️⃣ VM & AWS Setup  
- Launch **Ubuntu 24.04 VM** (t2.large, 28 GB, Name: `BMS-Server`).  
- Open required ports in Security Group:  
  - `22` (SSH), `25` (SMTP), `80` (HTTP), `443` (HTTPS), `465` (SMTPS),  
  - `8080` (Jenkins), `3000-10000` (Node.js, Grafana, custom apps),  
  - `6443` (Kubernetes API), `30000-32767` (K8s NodePort).  
- Create **IAM User** with permissions:  
  - `AmazonEC2FullAccess`, `AmazonEKSClusterPolicy`, `AmazonEKSWorkerNodePolicy`, `IAMFullAccess`  
- Install **AWS CLI** & configure access:  
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws configure
```
### 2️⃣ Install Jenkins
# Install JDK 17 & Jenkins
```
sudo apt install openjdk-17-jre-headless -y
wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```

  * Open Jenkins at: http://YOUR-IP:8080

  * Install plugins: NodeJS, SonarQube Scanner, Docker Pipeline, OWASP Dependency-Check, Email Extension

 ### 3️⃣ Install Docker
 ```
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo chmod 666 /var/run/docker.sock
docker login -u <DockerHub-Username>
```
### 4️⃣ Install Trivy
```
sudo apt-get install wget apt-transport-https gnupg -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
trivy --version
```
### 5️⃣ Setup SonarQube
```
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
* Open SonarQube at: http://YOUR-IP:9000
* 
* Default login: admin/admin

* Change password & generate SonarQube Token

* Configure SonarQube in Jenkins (Manage Jenkins → Configure System → SonarQube servers).

### 6️⃣ Email Integration

* Generate Gmail App Password (Google Account → Security → App Passwords).

* In Jenkins:

  * Add credentials (Username = Gmail ID, Password = App password).

  * Configure SMTP:

    * Server: smtp.gmail.com

    * Port: 465

    * SSL: ✅

* Now Jenkins will send build success/failure emails with logs attached.

## 🏗 Jenkins Pipeline
🔹 1. Clean Workspace
```
stage('Clean Workspace') {
    steps { cleanWs() }
}
```

* Removes old files to ensure a fresh build.

🔹 2. Checkout from GitHub
```
stage('Checkout from Git') {
    steps {
        git branch: 'main', url: 'https://github.com/KastroVKiran/Book-My-Show.git'
        sh 'ls -la'
    }
}
```


* Fetches the source code from GitHub.

🔹 3. SonarQube Analysis
```
stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('sonar-server') {
            sh '''
            $SCANNER_HOME/bin/sonar-scanner \
              -Dsonar.projectName=BMS \
              -Dsonar.projectKey=BMS
            '''
        }
    }
}
```


* Runs SonarQube static analysis.

🔹 4. Quality Gate Check
```
stage('Quality Gate') {
    steps {
        script {
            waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
        }
    }
}
```


* Waits for SonarQube to pass the quality gate.

🔹 5. Install Dependencies
```
stage('Install Dependencies') {
    steps {
        sh '''
        cd bookmyshow-app
        rm -rf node_modules package-lock.json
        npm install
        '''
    }
}
```


* Cleans & installs Node.js dependencies.

🔹 6. OWASP Dependency Scan
```
stage('OWASP FS Scan') {
    steps {
        dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit',
                        odcInstallation: 'DP-Check'
        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
    }
}
```

* Scans for dependency vulnerabilities.

🔹 7. Trivy FS Scan
```
stage('Trivy FS Scan') {
    steps { sh 'trivy fs . > trivyfs.txt' }
}
```

* Scans filesystem for vulnerabilities with Trivy.

🔹 8. Docker Build & Push
```
stage('Docker Build & Push') {
    steps {
        script {
            withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                sh '''
                docker build --no-cache -t moizaman/bms:latest -f bookmyshow-app/Dockerfile bookmyshow-app
                docker push moizaman/bms:latest
                '''
            }
        }
    }
}
```

* Builds Docker image & pushes to DockerHub.

🔹 9. Deploy to Container
```
stage('Deploy to Container') {
    steps {
        sh '''
        docker stop bms || true
        docker rm bms || true
        docker run -d --restart=always --name bms -p 3000:3000 moizaman/bms:latest
        '''
    }
}
```

* Deploys the app as a Docker container on port 3000.

🔹 10. Email Notifications
```
post {
    always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>Build Number: ${env.BUILD_NUMBER}<br/>URL: ${env.BUILD_URL}<br/>",
            to: 'moizansari.india@gmail.com',
            attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
    }
}
```

* Sends an email notification with logs and scan results.









