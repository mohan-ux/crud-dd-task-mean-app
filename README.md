# 🚀 MEAN Stack CRUD Application - DevOps Deployment

[![CI/CD Pipeline](https://github.com/mohan-ux/crud-dd-task-mean-app/actions/workflows/deploy.yml/badge.svg)](https://github.com/mohan-ux/crud-dd-task-mean-app/actions/workflows/deploy.yml)
[![Docker](https://img.shields.io/badge/Docker-Enabled-blue?logo=docker)](https://hub.docker.com/u/mohanux)
[![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws)](http://98.130.135.235)

A full-stack CRUD application built with **MongoDB, Express.js, Angular 15, and Node.js (MEAN Stack)**. This project demonstrates containerization with Docker, cloud deployment on AWS EC2, and automated CI/CD pipeline using GitHub Actions.

## 🌐 Live Demo

**Application URL:** [http://98.130.135.235](http://98.130.135.235)

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Local Development](#-local-development)
- [Docker Deployment](#-docker-deployment)
- [Cloud Deployment (AWS EC2)](#-cloud-deployment-aws-ec2)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Screenshots](#-screenshots)
- [API Endpoints](#-api-endpoints)
- [Troubleshooting](#-troubleshooting)
- [Author](#-author)

---

## ✨ Features

- ✅ **CRUD Operations** - Create, Read, Update, Delete tutorials
- ✅ **Search Functionality** - Search tutorials by title
- ✅ **Publish/Unpublish** - Toggle tutorial published status
- ✅ **RESTful API** - Express.js backend with MongoDB
- ✅ **Responsive UI** - Angular 15 with Bootstrap
- ✅ **Containerized** - Docker & Docker Compose
- ✅ **Cloud Deployed** - AWS EC2 Ubuntu instance
- ✅ **Reverse Proxy** - Nginx for routing
- ✅ **Automated CI/CD** - GitHub Actions pipeline

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | Angular 15, TypeScript, Bootstrap 4 |
| **Backend** | Node.js, Express.js |
| **Database** | MongoDB 6.0 |
| **Containerization** | Docker, Docker Compose |
| **Reverse Proxy** | Nginx |
| **Cloud Platform** | AWS EC2 (Ubuntu 22.04) |
| **CI/CD** | GitHub Actions |
| **Container Registry** | Docker Hub |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Port 80 (HTTP)                          │
│                           │                                 │
│                    ┌──────▼──────┐                          │
│                    │    Nginx    │                          │
│                    │   (Proxy)   │                          │
│                    └──────┬──────┘                          │
│                           │                                 │
│              ┌────────────┼────────────┐                    │
│              │            │            │                    │
│       ┌──────▼──────┐ ┌───▼───┐ ┌──────▼──────┐            │
│       │  Frontend   │ │  /api │ │   Backend   │            │
│       │  (Angular)  │ │  ───► │ │  (Node.js)  │            │
│       │   :80       │ │       │ │   :8080     │            │
│       └─────────────┘ └───────┘ └──────┬──────┘            │
│                                        │                    │
│                                 ┌──────▼──────┐            │
│                                 │   MongoDB   │            │
│                                 │   :27017    │            │
│                                 └─────────────┘            │
└─────────────────────────────────────────────────────────────┘
```

## 📦 Prerequisites

### For Local Development
- Node.js 18+
- MongoDB 6.0+
- Angular CLI 15+

### For Docker Deployment
- Docker Engine 20+
- Docker Compose V2

### For Cloud Deployment
- Ubuntu 22.04 VM (AWS/Azure/GCP)
- Docker & Docker Compose installed on VM
- SSH access to VM

## 🛠️ Local Development

### Backend Setup

```bash
cd backend
npm install
```

Update MongoDB connection in `app/config/db.config.js`:
```javascript
module.exports = {
  url: "mongodb://localhost:27017/dd_db"
};
```

Start the backend server:
```bash
node server.js
```

The API will be available at `http://localhost:8080`

### Frontend Setup

```bash
cd frontend
npm install
ng serve --port 8081
```

Navigate to `http://localhost:8081`

## 🐳 Docker Deployment

### Build and Run Locally with Docker Compose

1. **Clone the repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/crud-dd-task-mean-app.git
   cd crud-dd-task-mean-app
   ```

2. **Build and start all services:**
   ```bash
   docker-compose up -d --build
   ```

3. **Access the application:**
   - Open your browser and navigate to `http://localhost`

4. **View logs:**
   ```bash
   docker-compose logs -f
   ```

5. **Stop the application:**
   ```bash
   docker-compose down
   ```

### Build and Push Docker Images to Docker Hub

```bash
# Login to Docker Hub
docker login

# Build and push backend image
docker build -t YOUR_DOCKER_USERNAME/mean-backend:latest ./backend
docker push YOUR_DOCKER_USERNAME/mean-backend:latest

# Build and push frontend image
docker build -t YOUR_DOCKER_USERNAME/mean-frontend:latest ./frontend
docker push YOUR_DOCKER_USERNAME/mean-frontend:latest
```

## ☁️ Cloud Deployment (Ubuntu VM)

### Step 1: Set Up Ubuntu VM

1. Create an Ubuntu 22.04 VM on AWS/Azure/GCP
2. Open the following ports in the security group/firewall:
   - Port 22 (SSH)
   - Port 80 (HTTP)

### Step 2: Install Docker on VM

SSH into your VM and run:

```bash
# Update packages
sudo apt-get update

# Install Docker
sudo apt-get install -y docker.io

# Install Docker Compose
sudo apt-get install -y docker-compose-plugin

# Add current user to docker group
sudo usermod -aG docker $USER

# Apply group changes (or logout/login)
newgrp docker

# Verify installation
docker --version
docker compose version
```

### Step 3: Deploy the Application

```bash
# Create application directory
sudo mkdir -p /opt/mean-app
sudo chown $USER:$USER /opt/mean-app
cd /opt/mean-app

# Clone the repository (or copy files)
git clone https://github.com/YOUR_USERNAME/crud-dd-task-mean-app.git .

# Set environment variables
export DOCKER_USERNAME=your_docker_username
export TAG=latest

# Start the application
docker compose up -d

# Check running containers
docker compose ps

# View logs
docker compose logs -f
```

### Step 4: Access the Application

Open your browser and navigate to:
```
http://YOUR_VM_PUBLIC_IP
```

## 🔄 CI/CD Pipeline Setup

### GitHub Repository Secrets

Configure the following secrets in your GitHub repository:
- `Settings` → `Secrets and variables` → `Actions` → `New repository secret`

| Secret Name | Description |
|-------------|-------------|
| `DOCKER_USERNAME` | Your Docker Hub username |
| `DOCKER_PASSWORD` | Your Docker Hub password or access token |
| `VM_HOST` | Public IP address of your Ubuntu VM |
| `VM_USERNAME` | SSH username (usually `ubuntu` for AWS) |
| `VM_SSH_KEY` | Private SSH key for VM access |
| `VM_PORT` | SSH port (default: 22) |

### How the Pipeline Works

1. **On Push to `main` or `master` branch:**
   - Checks out the code
   - Builds Docker images for frontend and backend
   - Pushes images to Docker Hub with `latest` and commit SHA tags
   
2. **Deployment:**
   - Connects to VM via SSH
   - Pulls the latest Docker images
   - Restarts containers with updated images
   - Cleans up old Docker images

### Generate SSH Key for GitHub Actions

```bash
# Generate a new SSH key pair
ssh-keygen -t rsa -b 4096 -C "github-actions" -f ~/.ssh/github_actions_key -N ""

# Add public key to VM authorized_keys
cat ~/.ssh/github_actions_key.pub >> ~/.ssh/authorized_keys

# Copy the private key content to GitHub secrets (VM_SSH_KEY)
cat ~/.ssh/github_actions_key
```

## 📸 Screenshots

### 1. CI/CD Pipeline - GitHub Actions

![CI/CD Pipeline](screenshots/Screenshot%202025-11-28%20133608.png)
*GitHub Actions workflow showing successful build and deployment pipeline*

### 2. Docker Hub - Container Images

![Docker Hub Images](screenshots/Screenshot%202025-11-28%20133650.png)
*Docker images (mean-backend & mean-frontend) pushed to Docker Hub registry*

### 3. Application UI - Working Demo

![Application UI](screenshots/Screenshot%202025-11-28%20133733.png)
*MEAN Stack CRUD application running in browser - Tutorial management interface*

### 4. Docker Containers & Infrastructure

![Docker Containers](screenshots/Screenshot%202025-11-28%20133858.png)
*Docker containers running on AWS EC2 with Nginx reverse proxy*

---

## 📁 Project Structure

```
crud-dd-task-mean-app/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions CI/CD pipeline
├── backend/
│   ├── Dockerfile              # Backend Docker configuration
│   ├── .dockerignore           # Docker ignore file
│   ├── package.json            # Node.js dependencies
│   ├── server.js               # Express server entry point
│   └── app/
│       ├── config/
│       │   └── db.config.js    # MongoDB configuration
│       ├── controllers/
│       │   └── tutorial.controller.js
│       ├── models/
│       │   ├── index.js
│       │   └── tutorial.model.js
│       └── routes/
│           └── turorial.routes.js
├── frontend/
│   ├── Dockerfile              # Frontend Docker configuration
│   ├── .dockerignore           # Docker ignore file
│   ├── nginx.conf              # Nginx config for Angular
│   ├── package.json            # Angular dependencies
│   ├── angular.json            # Angular configuration
│   └── src/
│       ├── app/
│       │   ├── components/     # Angular components
│       │   ├── models/         # Data models
│       │   └── services/       # API services
│       └── environments/       # Environment configs
├── nginx/
│   └── nginx.conf              # Main Nginx reverse proxy config
├── docker-compose.yml          # Docker Compose configuration
└── README.md                   # This file
```

## 🔧 Troubleshooting

### Common Issues

1. **MongoDB connection failed:**
   ```bash
   # Check if MongoDB container is running
   docker compose logs mongodb
   
   # Restart MongoDB
   docker compose restart mongodb
   ```

2. **Frontend not loading:**
   ```bash
   # Check frontend container logs
   docker compose logs frontend
   
   # Rebuild frontend image
   docker compose build frontend
   docker compose up -d frontend
   ```

3. **API requests failing:**
   ```bash
   # Check backend container logs
   docker compose logs backend
   
   # Check nginx proxy logs
   docker compose logs nginx
   ```

4. **Port 80 already in use:**
   ```bash
   # Find process using port 80
   sudo lsof -i :80
   
   # Stop the conflicting service
   sudo systemctl stop nginx  # or apache2
   ```

### Useful Commands

```bash
# View all container logs
docker compose logs -f

# Restart all services
docker compose restart

# Rebuild and restart specific service
docker compose up -d --build backend

# Check container health
docker compose ps

# Remove all containers and volumes
docker compose down -v

# SSH into a container
docker exec -it mean-backend sh
```

---

## 📄 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/tutorials` | Get all tutorials |
| `GET` | `/api/tutorials/:id` | Get tutorial by ID |
| `POST` | `/api/tutorials` | Create new tutorial |
| `PUT` | `/api/tutorials/:id` | Update tutorial |
| `DELETE` | `/api/tutorials/:id` | Delete tutorial |
| `DELETE` | `/api/tutorials` | Delete all tutorials |
| `GET` | `/api/tutorials?title=keyword` | Search by title |
| `GET` | `/api/tutorials/published` | Get published tutorials |

---

## 🚀 Deployment Summary

| Component | Details |
|-----------|---------|
| **Cloud Provider** | AWS EC2 |
| **Instance Type** | t3.micro |
| **OS** | Ubuntu 22.04 LTS |
| **Public IP** | 98.130.135.235 |
| **Application URL** | http://98.130.135.235 |
| **Docker Hub** | [mohanux](https://hub.docker.com/u/mohanux) |
| **GitHub Repo** | [mohan-ux/crud-dd-task-mean-app](https://github.com/mohan-ux/crud-dd-task-mean-app) |

---

## 📦 Docker Images

| Image | Description | Registry |
|-------|-------------|----------|
| `mohanux/mean-backend:latest` | Node.js Express API | [Docker Hub](https://hub.docker.com/r/mohanux/mean-backend) |
| `mohanux/mean-frontend:latest` | Angular 15 App | [Docker Hub](https://hub.docker.com/r/mohanux/mean-frontend) |

---

## 🔄 CI/CD Pipeline Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   GitHub    │────▶│   Build     │────▶│  Push to    │────▶│  Deploy to  │
│   Push      │     │   Images    │     │  Docker Hub │     │   AWS EC2   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
      │                   │                   │                   │
      ▼                   ▼                   ▼                   ▼
  Trigger on         Build frontend      Push images         SSH into VM
  main branch        & backend           with tags           Pull & restart
```

---

## 👤 Author

**Mohan**

- GitHub: [@mohan-ux](https://github.com/mohan-ux)
- Docker Hub: [mohanux](https://hub.docker.com/u/mohanux)

---

## 📝 License

This project is licensed under the ISC License.

---

## 🙏 Acknowledgments

- DevOps assignment for demonstrating containerization, CI/CD, and cloud deployment skills
- Built with MEAN Stack (MongoDB, Express, Angular, Node.js)

---

⭐ **If you found this project helpful, please give it a star!**
