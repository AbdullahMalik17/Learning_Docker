# Docker Learning Path: Beginner to Advanced

## 🎯 Learning Objectives
This guide will take you from Docker basics to advanced concepts, specifically designed for students learning Docker on WSL (Windows Subsystem for Linux).

## 📚 Prerequisites
- WSL installed (`wsl --install`)
- Basic Linux command line knowledge
- Text editor (nano/vim) familiarity

---

## 🚀 Phase 1: Docker Fundamentals (Week 1-2)

### 1.1 WSL Docker Setup

#### Install Docker on WSL
```bash
# Update package list
sudo apt update

# Install Docker
sudo apt install docker.io

# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Add your user to docker group (avoid using sudo)
sudo usermod -aG docker $USER

# Logout and login again, then test
docker --version
```

#### Verify Installation
```bash
# Check Docker version
docker --version

# Check Docker info
docker info

# Test Docker with hello-world
docker run hello-world
```

### 1.2 Basic Docker Concepts

#### Understanding Docker Architecture
- **Docker Engine**: The core Docker application
- **Images**: Read-only templates for containers
- **Containers**: Running instances of images
- **Dockerfile**: Instructions to build images
- **Registry**: Storage for Docker images (Docker Hub)

#### Essential Commands Practice
```bash
# 1. Pull your first image
docker pull nginx

# 2. List images
docker images

# 3. Run a container
docker run -d --name my-first-container nginx

# 4. List running containers
docker ps

# 5. View container logs
docker logs my-first-container

# 6. Stop the container
docker stop my-first-container

# 7. Remove the container
docker rm my-first-container
```

### 1.3 Hands-on Exercise 1: Basic Container Management

#### Exercise: Web Server Setup
```bash
# Create a project directory
mkdir ~/docker-learning
cd ~/docker-learning

# Pull nginx image
docker pull nginx

# Run nginx container with port mapping
docker run -d --name web-server -p 8080:80 nginx

# Test the web server
curl http://localhost:8080

# Check container status
docker ps

# View container logs
docker logs web-server

# Stop and remove container
docker stop web-server
docker rm web-server
```

---

## 🔧 Phase 2: Docker Images and Dockerfiles (Week 3-4)

### 2.1 Understanding Docker Images

#### Image Layers and Caching
- Images are built in layers
- Each instruction in Dockerfile creates a layer
- Layers are cached for faster builds
- Understanding image size optimization

#### Working with Images
```bash
# Search for images
docker search ubuntu

# Pull specific version
docker pull ubuntu:20.04

# Inspect image details
docker inspect ubuntu:20.04

# Remove unused images
docker image prune
```

### 2.2 Creating Your First Dockerfile

#### Basic Dockerfile Structure
```dockerfile
# Use official base image
FROM ubuntu:20.04

# Set maintainer
LABEL maintainer="student@example.com"

# Update package list
RUN apt-get update

# Install required packages
RUN apt-get install -y python3 python3-pip

# Set working directory
WORKDIR /app

# Copy application files
COPY . .

# Install Python dependencies
RUN pip3 install -r requirements.txt

# Expose port
EXPOSE 5000

# Set default command
CMD ["python3", "app.py"]
```

### 2.3 Hands-on Exercise 2: Simple Python Web App

#### Create Project Structure
```bash
# Create project directory
mkdir ~/docker-learning/python-app
cd ~/docker-learning/python-app

# Create Python application
cat > app.py << 'EOF'
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
EOF

# Create requirements file
cat > requirements.txt << 'EOF'
Flask==2.0.1
EOF
```

#### Create Dockerfile
```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

#### Build and Run
```bash
# Build the image
docker build -t my-python-app .

# Run the container
docker run -d --name python-web -p 5000:5000 my-python-app

# Test the application
curl http://localhost:5000

# Check logs
docker logs python-web
```

---

## 🏗️ Phase 3: Docker Compose and Multi-Container Apps (Week 5-6)

### 3.1 Introduction to Docker Compose

#### What is Docker Compose?
- Tool for defining and running multi-container applications
- Uses YAML files to configure services
- Simplifies complex application deployment

#### Basic docker-compose.yml Structure
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### 3.2 Hands-on Exercise 3: Full-Stack Application

#### Project Structure
```
fullstack-app/
├── docker-compose.yml
├── web/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
└── database/
    └── init.sql
```

#### Create Web Application
```bash
mkdir ~/docker-learning/fullstack-app
cd ~/docker-learning/fullstack-app

# Create web directory
mkdir web
cd web

# Create Flask app with database
cat > app.py << 'EOF'
from flask import Flask, render_template_string
import psycopg2
import os

app = Flask(__name__)

def get_db_connection():
    conn = psycopg2.connect(
        host='db',
        database='myapp',
        user='user',
        password='password'
    )
    return conn

@app.route('/')
def index():
    conn = get_db_connection()
    cur = conn.cursor()
    cur.execute('SELECT * FROM users;')
    users = cur.fetchall()
    cur.close()
    conn.close()
    
    return f"Users: {users}"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
EOF

# Create requirements
cat > requirements.txt << 'EOF'
Flask==2.0.1
psycopg2-binary==2.9.1
EOF

# Create Dockerfile
cat > Dockerfile << 'EOF'
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
EOF
```

#### Create Docker Compose File
```bash
cd ~/docker-learning/fullstack-app

cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "5000:5000"
    depends_on:
      - db
    environment:
      - FLASK_ENV=development

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
EOF
```

#### Run the Application
```bash
# Start all services
docker-compose up -d

# Check running services
docker-compose ps

# View logs
docker-compose logs

# Test the application
curl http://localhost:5000

# Stop services
docker-compose down
```

---

## 🌐 Phase 4: Docker Networking and Volumes (Week 7-8)

### 4.1 Docker Networking

#### Network Types
- **Bridge**: Default network for containers
- **Host**: Use host's network directly
- **None**: No networking
- **Overlay**: For multi-host networking

#### Network Commands
```bash
# List networks
docker network ls

# Create custom network
docker network create my-network

# Run container on specific network
docker run --network my-network nginx

# Inspect network
docker network inspect my-network
```

### 4.2 Docker Volumes

#### Volume Types
- **Named Volumes**: Managed by Docker
- **Bind Mounts**: Mount host directories
- **tmpfs Mounts**: In-memory storage

#### Volume Commands
```bash
# List volumes
docker volume ls

# Create volume
docker volume create my-volume

# Use volume in container
docker run -v my-volume:/data nginx

# Inspect volume
docker volume inspect my-volume
```

### 4.3 Hands-on Exercise 4: Data Persistence

#### Create Database with Persistent Storage
```bash
# Create volume
docker volume create postgres-data

# Run PostgreSQL with volume
docker run -d \
  --name postgres-db \
  -e POSTGRES_DB=testdb \
  -e POSTGRES_USER=testuser \
  -e POSTGRES_PASSWORD=testpass \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:13

# Connect to database
docker exec -it postgres-db psql -U testuser -d testdb

# Create table and insert data
CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(100));
INSERT INTO users (name) VALUES ('John'), ('Jane');
SELECT * FROM users;
\q

# Stop and remove container
docker stop postgres-db
docker rm postgres-db

# Run new container with same volume
docker run -d \
  --name postgres-db-new \
  -e POSTGRES_DB=testdb \
  -e POSTGRES_USER=testuser \
  -e POSTGRES_PASSWORD=testpass \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:13

# Verify data persistence
docker exec -it postgres-db-new psql -U testuser -d testdb -c "SELECT * FROM users;"
```

---

## 🔒 Phase 5: Docker Security and Best Practices (Week 9-10)

### 5.1 Security Best Practices

#### Image Security
```bash
# Use specific tags instead of latest
docker pull nginx:1.21

# Scan images for vulnerabilities
docker scan nginx:1.21

# Use official images
docker pull postgres:13
```

#### Container Security
```bash
# Run as non-root user
docker run --user 1000:1000 nginx

# Set resource limits
docker run --memory=512m --cpus=1 nginx

# Use read-only filesystem
docker run --read-only nginx
```

### 5.2 Multi-stage Builds

#### Optimized Dockerfile
```dockerfile
# Build stage
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 5.3 Hands-on Exercise 5: Secure Multi-stage Build

#### Create Node.js Application
```bash
mkdir ~/docker-learning/secure-app
cd ~/docker-learning/secure-app

# Create package.json
cat > package.json << 'EOF'
{
  "name": "secure-app",
  "version": "1.0.0",
  "scripts": {
    "build": "echo 'Building app...' && mkdir -p dist && echo 'Hello World' > dist/index.html"
  }
}
EOF

# Create multi-stage Dockerfile
cat > Dockerfile << 'EOF'
# Build stage
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001
COPY --from=builder /app/dist /usr/share/nginx/html
USER nextjs
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
EOF

# Build and run
docker build -t secure-app .
docker run -d --name secure-web -p 8080:80 secure-app
```

---

## 🚀 Phase 6: Advanced Docker Topics (Week 11-12)

### 6.1 Docker Swarm (Basic Orchestration)

#### Initialize Swarm
```bash
# Initialize swarm
docker swarm init

# Join worker nodes (if available)
docker swarm join --token <token> <manager-ip>:2377

# Deploy stack
docker stack deploy -c docker-compose.yml myapp

# List services
docker service ls
```

### 6.2 Container Monitoring and Logging

#### Monitoring Commands
```bash
# Monitor resource usage
docker stats

# Monitor events
docker events

# System information
docker system df
docker system info
```

#### Logging Best Practices
```bash
# Configure log driver
docker run --log-driver=json-file --log-opt max-size=10m nginx

# Centralized logging with ELK stack
# (Advanced topic - requires additional setup)
```

### 6.3 Hands-on Exercise 6: Production-Ready Setup

#### Create Production Docker Compose
```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "80:5000"
    environment:
      - FLASK_ENV=production
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000"]
      interval: 30s
      timeout: 10s
      retries: 3

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d myapp"]
      interval: 30s
      timeout: 10s
      retries: 3

  nginx:
    image: nginx:alpine
    ports:
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - web
    restart: unless-stopped

volumes:
  postgres_data:
```

---

## 📊 Progress Tracking

### Week-by-Week Checklist

#### Week 1-2: Fundamentals ✅
- [ ] Docker installed on WSL
- [ ] First container run
- [ ] Basic commands mastered
- [ ] Container lifecycle understood

#### Week 3-4: Images and Dockerfiles ✅
- [ ] First Dockerfile created
- [ ] Image built successfully
- [ ] Multi-layer builds understood
- [ ] Image optimization practiced

#### Week 5-6: Docker Compose ✅
- [ ] Multi-container app created
- [ ] Docker Compose file written
- [ ] Service dependencies configured
- [ ] Full-stack application deployed

#### Week 7-8: Networking and Volumes ✅
- [ ] Custom networks created
- [ ] Data persistence implemented
- [ ] Volume management practiced
- [ ] Network isolation understood

#### Week 9-10: Security and Best Practices ✅
- [ ] Security best practices applied
- [ ] Multi-stage builds implemented
- [ ] Resource limits set
- [ ] Non-root containers used

#### Week 11-12: Advanced Topics ✅
- [ ] Docker Swarm basics learned
- [ ] Monitoring implemented
- [ ] Production-ready setup created
- [ ] Logging configured

---

## 🎯 Final Project: Complete Application

### Project Requirements
Create a complete web application with:
- Frontend (React/Vue/Angular)
- Backend API (Node.js/Python/Java)
- Database (PostgreSQL/MySQL)
- Reverse Proxy (Nginx)
- Monitoring and Logging
- Security best practices
- CI/CD pipeline (bonus)

### Deliverables
1. Source code repository
2. Dockerfiles for all services
3. Docker Compose configuration
4. Documentation
5. Deployment guide

---

## 📚 Additional Resources

### Official Documentation
- [Docker Official Docs](https://docs.docker.com/)
- [Docker Compose Reference](https://docs.docker.com/compose/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/dev-best-practices/)

### Learning Platforms
- [Docker Labs](https://labs.play-with-docker.com/)
- [Katacoda Docker Scenarios](https://www.katacoda.com/courses/docker)
- [Docker Training](https://training.docker.com/)

### Practice Projects
1. **Blog Platform**: WordPress + MySQL
2. **E-commerce**: Node.js + MongoDB + Redis
3. **Microservices**: Multiple services with API Gateway
4. **DevOps Pipeline**: CI/CD with Docker

---

## 🏆 Certification Path

### Docker Certifications
1. **Docker Certified Associate (DCA)**
2. **Kubernetes Certifications** (next step)
3. **Cloud Platform Certifications**

### Preparation Resources
- Official Docker training courses
- Practice exams
- Hands-on labs
- Community forums

---

## 💡 Tips for Success

### Daily Practice
- Spend 30-60 minutes daily on Docker
- Practice commands regularly
- Build small projects weekly
- Read Docker documentation

### Community Engagement
- Join Docker community forums
- Participate in Docker meetups
- Contribute to open-source projects
- Share your learning journey

### Career Development
- Build a portfolio of Docker projects
- Document your learning process
- Create tutorials for others
- Apply Docker skills in real projects

---

