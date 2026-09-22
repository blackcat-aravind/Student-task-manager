# Student Task Manager – GitHub, Jenkins & Docker CI/CD

A beginner-friendly DevOps project demonstrating CI/CD workflow using GitHub, Jenkins, and Docker.

## 📋 Description

This is a simple web-based Student Task Manager application that helps students organize their assignments and projects. The application is containerized using Docker and automated using Jenkins CI/CD pipeline.

## ✨ Features

- ➕ Add new tasks
- ✅ Mark tasks as completed
- ❌ Delete tasks
- 📊 View statistics (Total, Completed, Pending tasks)
- 💾 Persistent storage using browser localStorage
- ⌨️ Press Enter to add tasks quickly
- 📱 Fully responsive design for mobile and desktop
- 🎨 Clean, professional UI without excessive styling

## 🛠️ Technologies Used

- **HTML** - Structure of the web application
- **CSS** - Styling and responsive design
- **JavaScript** - Application logic and localStorage
- **Nginx** - Web server to serve static files
- **Docker** - Containerization of the application
- **Jenkins** - CI/CD automation
- **Git/GitHub** - Version control and source code management

## 📁 Project Structure

```
student-task-manager/
├── index.html      # Main HTML file
├── style.css       # Stylesheet
├── script.js       # JavaScript logic
├── Dockerfile      # Docker configuration
├── Jenkinsfile     # Jenkins pipeline
├── README.md       # Project documentation
└── .gitignore      # Git ignore file
```

## 🖥️ How to Run Locally

Simply open `index.html` in your web browser:

```bash
# Windows
start index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

## 🐳 Docker Commands

### Build Docker Image

```bash
docker build -t student-task-manager .
```

### Run Docker Container

```bash
docker run -d -p 8080:80 --name student-task-manager student-task-manager
```

### Access the Application

Open your browser and go to:
```
http://localhost:8080
```

### Stop the Container

```bash
docker stop student-task-manager
```

### Remove the Container

```bash
docker rm student-task-manager
```

### View Running Containers

```bash
docker ps
```

### View Container Logs

```bash
docker logs student-task-manager
```

## 📦 How GitHub is Used

GitHub serves as the **source code repository** and **version control system** for this project:

- Stores all project files
- Tracks changes and version history
- Enables collaboration
- Integrates with Jenkins for automated builds
- Provides backup and restore capabilities

## 🔄 How Jenkins is Used

Jenkins is the **CI/CD automation server** that:

- Automatically pulls code from GitHub when changes are detected
- Validates that all required files exist
- Builds the Docker image
- Deploys the application in a Docker container
- Provides build status and logs
- Automates the entire deployment workflow

## 🐋 How Docker is Used

Docker provides **containerization**:

- Packages the application with all dependencies
- Ensures consistent environment across different machines
- Uses Nginx web server to serve the application
- Simplifies deployment and scaling
- Isolates the application from the host system

## 🔁 CI/CD Workflow

```
Developer
   ↓
Git (local commits)
   ↓
GitHub (push code)
   ↓
Jenkins (webhook triggers build)
   ↓
Validate (check files exist)
   ↓
Docker Build (create image)
   ↓
Docker Container (deploy)
   ↓
Application Running (localhost:8080)
```

## ⚙️ Jenkins Setup Instructions

### 1. Install Jenkins

Download and install Jenkins from [jenkins.io](https://www.jenkins.io/download/)

### 2. Install Required Plugins

- Go to **Manage Jenkins** → **Manage Plugins**
- Install:
  - Git Plugin
  - Docker Pipeline Plugin
  - GitHub Integration Plugin

### 3. Configure Docker in Jenkins

- Go to **Manage Jenkins** → **Global Tool Configuration**
- Ensure Docker is accessible from Jenkins

### 4. Create a New Pipeline Job

1. Click **New Item**
2. Enter name: `student-task-manager-pipeline`
3. Select **Pipeline**
4. Click **OK**

### 5. Configure Pipeline

**General Section:**
- Add description: "CI/CD pipeline for Student Task Manager"

**Build Triggers:**
- Check **GitHub hook trigger for GITScm polling** (for automatic builds)
- OR check **Poll SCM** and enter: `H/5 * * * *` (check every 5 minutes)

**Pipeline Section:**
- Definition: **Pipeline script from SCM**
- SCM: **Git**
- Repository URL: `https://github.com/YOUR_USERNAME/student-task-manager.git`
- Branch: `*/main` (or `*/master`)
- Script Path: `Jenkinsfile`

### 6. Save and Build

- Click **Save**
- Click **Build Now** to test

## 📂 Git Commands to Push to GitHub

### 1. Initialize Git Repository

```bash
git init
```

### 2. Add All Files

```bash
git add .
```

### 3. Create First Commit

```bash
git commit -m "Initial commit: Student Task Manager with CI/CD"
```

### 4. Create GitHub Repository

- Go to [github.com](https://github.com)
- Click **New Repository**
- Name: `student-task-manager`
- **Do NOT** initialize with README (we already have files)
- Click **Create Repository**

### 5. Link Local Repository to GitHub

```bash
git remote add origin https://github.com/YOUR_USERNAME/student-task-manager.git
```

### 6. Push to GitHub

```bash
git branch -M main
git push -u origin main
```

### 7. Verify on GitHub

Check your repository on GitHub to confirm all files are uploaded.

### Future Updates

```bash
git add .
git commit -m "Description of changes"
git push
```

## ⚠️ Common Errors and Fixes

### Error: Port 8080 already in use

**Solution:**
```bash
# Find and stop the container using port 8080
docker ps
docker stop student-task-manager

# Or use a different port
docker run -d -p 8081:80 --name student-task-manager student-task-manager
```

### Error: Docker daemon not running

**Solution:**
- Windows/Mac: Start Docker Desktop
- Linux: `sudo systemctl start docker`

### Error: Permission denied (Jenkins)

**Solution:**
Add Jenkins user to docker group:
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

### Error: Container name already in use

**Solution:**
```bash
docker rm student-task-manager
```

### Jenkins build fails at checkout

**Solution:**
- Verify GitHub repository URL is correct
- Check GitHub credentials in Jenkins
- Ensure the repository is public or credentials are configured

## 🎓 Viva Questions and Answers

### 1. What is CI/CD?

**Answer:** CI/CD stands for Continuous Integration and Continuous Deployment. It automates the process of integrating code changes, testing them, and deploying to production.

### 2. What is Docker?

**Answer:** Docker is a containerization platform that packages applications with their dependencies into containers, ensuring they run consistently across different environments.

### 3. What is Jenkins?

**Answer:** Jenkins is an open-source automation server used for CI/CD. It automates building, testing, and deploying applications.

### 4. Why use Nginx in Docker?

**Answer:** Nginx is a lightweight, high-performance web server ideal for serving static files like HTML, CSS, and JavaScript. It's efficient and commonly used in production.

### 5. What is a Dockerfile?

**Answer:** A Dockerfile is a text file containing instructions to build a Docker image. It specifies the base image, files to copy, ports to expose, and commands to run.

### 6. What is a Jenkinsfile?

**Answer:** A Jenkinsfile defines the Jenkins pipeline as code. It contains stages like checkout, build, test, and deploy, making the CI/CD process reproducible.

### 7. What is the difference between Docker image and container?

**Answer:** A Docker **image** is a template (like a class), while a **container** is a running instance of that image (like an object).

### 8. What happens when you push code to GitHub?

**Answer:** Jenkins detects the change via webhook or polling, pulls the latest code, runs the pipeline (validate, build, deploy), and deploys the updated application.

### 9. What is localhost:8080?

**Answer:** localhost refers to your own computer. Port 8080 is where the Docker container exposes the application. So localhost:8080 accesses the app running in Docker.

### 10. Why use version control like Git?

**Answer:** Version control tracks changes, allows collaboration, provides backup, enables rollback to previous versions, and maintains a complete history of the project.

---

**Created with Docker 🐳 | Automated with Jenkins 🔄 | Managed with Git 📦**
