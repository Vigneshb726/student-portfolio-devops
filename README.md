# Student Portfolio – DevOps Project

A responsive student portfolio website (HTML + CSS) deployed using **GitHub, Docker, Docker Hub, Kubernetes and Jenkins**.

**Workflow:** VS Code → GitHub → Docker → Docker Hub → Kubernetes → Jenkins → Final Website

## Project Structure

```
index.html            Portfolio web page
style.css             Styles and responsive layout
Dockerfile            Builds an nginx image serving the site
.dockerignore         Files excluded from the image
Jenkinsfile           CI/CD pipeline (build → push → deploy → verify)
k8s/deployment.yaml   Kubernetes Deployment (2 replicas)
k8s/service.yaml      Kubernetes NodePort Service (port 30090)
```

## Run with Docker

```bash
docker build -t student-portfolio .
docker run -d -p 8080:80 --name portfolio-container student-portfolio
```

Open http://localhost:8080

## Push to Docker Hub

```bash
docker login
docker tag student-portfolio:latest vigneshb2006/student-portfolio:latest
docker push vigneshb2006/student-portfolio:latest
```

## Deploy to Kubernetes

```bash
kubectl get nodes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get pods
kubectl get svc
```

Open http://localhost:30090

## Jenkins

Create a **Pipeline** job that uses *Pipeline script from SCM* pointing to this repository.
Add a Docker Hub credential with the ID `dockerhub` (Username with password).
