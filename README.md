# MEAN Stack CRUD Application

A full-stack CRUD application built with MongoDB, Express.js, Angular 15, and Node.js. This application allows users to manage tutorials with create, read, update, and delete operations.

## 📋 Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Local Development](#local-development)
- [Docker Deployment](#docker-deployment)
- [Cloud Deployment (Ubuntu VM)](#cloud-deployment-ubuntu-vm)
- [CI/CD Pipeline Setup](#cicd-pipeline-setup)
- [Screenshots](#screenshots)
- [Troubleshooting](#troubleshooting)

## ✨ Features

- Create, Read, Update, Delete (CRUD) operations for tutorials
- Search tutorials by title
- Toggle published status
- RESTful API backend
- Responsive Angular frontend
- Dockerized deployment
- Nginx reverse proxy
- Automated CI/CD with GitHub Actions

## 🏗️ Architecture

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

### 1. CI/CD Configuration and Execution

![GitHub Actions Workflow](screenshots/github-actions-workflow.png)
*GitHub Actions workflow configuration and successful execution*

### 2. Docker Image Build and Push

![Docker Hub Images](screenshots/docker-hub-images.png)
*Docker images pushed to Docker Hub*

### 3. Application Deployment

![Running Containers](screenshots/docker-containers.png)
*Docker containers running on the VM*

### 4. Working Application UI

![Application UI](screenshots/application-ui.png)
*MEAN Stack CRUD application running in browser*

### 5. Nginx Setup and Infrastructure

![Nginx Configuration](screenshots/nginx-setup.png)
*Nginx reverse proxy configuration*

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
```

## 📄 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tutorials` | Get all tutorials |
| GET | `/api/tutorials/:id` | Get tutorial by ID |
| POST | `/api/tutorials` | Create new tutorial |
| PUT | `/api/tutorials/:id` | Update tutorial |
| DELETE | `/api/tutorials/:id` | Delete tutorial |
| DELETE | `/api/tutorials` | Delete all tutorials |
| GET | `/api/tutorials?title=keyword` | Search by title |

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the ISC License.

---

**Note:** Remember to replace `YOUR_USERNAME` and `YOUR_DOCKER_USERNAME` with your actual GitHub and Docker Hub usernames throughout this documentation.
