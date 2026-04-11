# my-devops-project
CI/CD trigger


password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ secrets.DOCKER_USERNAME }}/flask-devops-app:latest
            ${{ secrets.DOCKER_USERNAME }}/flask-devops-app:${{ github.sha }}

      - name: Done
        run: echo "Docker image pushed to Docker Hub successfully!"


---

## Commands I Used in This Project

### Git Commands

bash
# Initialize git in project folder
git init

# Check which files changed
git status

# Add all files to staging
git add .

# Add one specific file
git add app.py

# Save changes with a message
git commit -m "your message here"

# Connect to GitHub repo
git remote add origin https://github.com/asritha-peddi/my-devops-project.git

# Push code to GitHub (first time)
git push -u origin main

# Push code to GitHub (after first time)
git push origin main

# See all commits history
git log --oneline

# Pull latest code from GitHub
git pull origin main


### Docker Commands

bash
# Build Docker image from Dockerfile
docker build -t flask-devops-app .

# Run container on port 5000
docker run -p 5000:5000 flask-devops-app

# Run container in background (detached mode)
docker run -d -p 5000:5000 --name myapp flask-devops-app

# See all running containers
docker ps

# See all containers including stopped ones
docker ps -a

# Stop a running container
docker stop myapp

# Remove a container
docker rm myapp

# See all Docker images on your laptop
docker images

# Remove a Docker image
docker rmi flask-devops-app

# Pull image from Docker Hub
docker pull asrithapeddi/flask-devops-app:latest

# Push image to Docker Hub
docker push asrithapeddi/flask-devops-app:latest

# Login to Docker Hub from terminal
docker login

# See logs of a running container
docker logs myapp

# Go inside a running container (like SSH)
docker exec -it myapp bash


### Python Commands

bash
# Install packages from requirements.txt
pip install -r requirements.txt

# Run the Flask app locally
python app.py

# Check Python version
python --version

# Check pip version
pip --version

# See all installed packages
pip list

# Save installed packages to requirements.txt
pip freeze > requirements.txt


### GitHub Actions â€” How Pipeline Was Triggered

bash
# Every time you run this command, the pipeline starts automatically:
git add .
git commit -m "any message"
git push origin main

# You can watch it run here:
# https://github.com/asritha-peddi/my-devops-project/actions


---

## How to Run This Project Locally

### Option 1 â€” Run with Python directly

bash
# Step 1 â€” Clone the repository
git clone https://github.com/asritha-peddi/my-devops-project.git

# Step 2 â€” Go into the project folder
cd my-devops-project

# Step 3 â€” Install Python packages
pip install -r requirements.txt

# Step 4 â€” Run the app
python app.py

# Step 5 â€” Open browser and go to:
# http://localhost:5000
# http://localhost:5000/health


### Option 2 â€” Run with Docker

bash
# Step 1 â€” Clone the repository
git clone https://github.com/asritha-peddi/my-devops-project.git

# Step 2 â€” Go into the project folder
cd my-devops-project

# Step 3 â€” Build the Docker image
docker build -t flask-devops-app .

# Step 4 â€” Run the container
docker run -p 5000:5000 flask-devops-app

# Step 5 â€” Open browser and go to:
# http://localhost:5000
# http://localhost:5000/health


### Option 3 â€” Pull from Docker Hub (easiest)

bash
# Pull my image directly from Docker Hub
docker pull asrithapeddi/flask-devops-app:latest

# Run it
docker run -p 5000:5000 asrithapeddi/flask-devops-app:latest

# Open browser: http://localhost:5000


---

## API Endpoints

| Endpoint | Method | What it returns |
|----------|--------|-----------------|
| `/` | GET | Hello message and status |
| `/health` | GET | Health check â€” returns 200 OK |

### Test the endpoints

bash
# Test home endpoint
curl http://localhost:5000/

# Test health endpoint
curl http://localhost:5000/health

# Expected response from /health:
# {"status": "healthy"}


---

## CI/CD Pipeline Explanation

### Stage 1 â€” Test Job

runs-on: ubuntu-latest       â†’ GitHub gives a free Linux machine
actions/checkout@v4          â†’ Downloads your code
actions/setup-python@v5      â†’ Installs Python 3.11
pip install -r requirements  â†’ Installs Flask and Gunicorn
python app.py &              â†’ Starts Flask app in background
sleep 4                      â†’ Waits 4 seconds for app to start
curl -f http://localhost:5000/health  â†’ Tests if app is healthy


### Stage 2 â€” Build and Push Job

needs: test                  â†’ Only runs if Stage 1 passed
docker/login-action@v3       â†’ Logs in to Docker Hub using secrets
docker/build-push-action@v5  â†’ Builds image from Dockerfile
push: true                   â†’ Pushes to Docker Hub
tags: latest + commit SHA    â†’ Two tags for every image


### GitHub Secrets Used

DOCKER_USERNAME  â†’ Your Docker Hub username (stored safely in GitHub)
DOCKER_PASSWORD  â†’ Your Docker Hub password (never shown in logs)
```

---

## What I Learned

- How to containerize a Python Flask app using Docker
- How to write a multi-stage CI/CD pipeline using GitHub Actions
- How to store secrets safely using GitHub Secrets
- How to push Docker images to Docker Hub automatically
- How Docker layer caching works to speed up builds
- Git branching and commit best practices
- How health check endpoints work in production apps

---
