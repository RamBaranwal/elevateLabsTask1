# Node.js Demo App — CI/CD Pipeline

## Elevate Labs DevOps Internship — Task 1

This project demonstrates an automated CI/CD pipeline for a Node.js web application using GitHub Actions and Docker.

The pipeline automatically:

1. Installs project dependencies
2. Runs automated tests
3. Builds a Docker image
4. Logs in to Docker Hub
5. Pushes the Docker image to Docker Hub

### CI/CD Flow

```text
Developer
    |
    | git push origin main
    v
GitHub Repository
    |
    v
GitHub Actions
    |
    v
+----------------------+
| Test Application     |
|                      |
| npm ci               |
| npm test             |
+----------+-----------+
           |
           | Tests pass
           v
+----------------------+
| Build & Push Docker  |
| Image                |
|                      |
| docker build         |
| docker login         |
| docker push          |
+----------+-----------+
           |
           v
       Docker Hub