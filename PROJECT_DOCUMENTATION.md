# DevOps Internship – Task 1
# Automate Code Deployment Using CI/CD Pipeline

## 1. Project Overview

This project was completed for the DevOps Internship Task 1.

The task is to create a Node.js web application and automate its build, test, Docker image creation, and deployment workflow using GitHub Actions.

The project uses:

- Node.js
- Express
- Jest
- Supertest
- Docker
- Docker Hub
- Git
- GitHub
- GitHub Actions

The application is named:

```text
nodejs-demo-app
```

Project location used during development:

```text
D:\elevateLab\nodejs-demo-app
```

---

## 2. Task Objective

The objective of this task is:

> Set up a CI/CD pipeline to build and deploy a web application.

The workflow is designed around the following process:

```text
Developer
   |
   v
GitHub Repository
   |
   v
GitHub Actions
   |
   +----> Install dependencies
   |
   +----> Run tests
   |
   +----> Build Docker image
   |
   +----> Push Docker image to Docker Hub
   |
   v
Deployment-ready Docker image
```

The internship task specifically requires a GitHub repository containing a `.yml` GitHub Actions workflow and recommends automating the test → build → push process.

---

## 3. Tools and Technologies Used

| Tool / Technology | Purpose |
|---|---|
| Node.js | Runtime for the web application |
| Express | Web application framework |
| npm | Package and dependency management |
| Jest | Automated testing |
| Supertest | HTTP/API testing |
| Docker | Containerization |
| Docker Hub | Docker image registry |
| Git | Version control |
| GitHub | Remote source-code repository |
| GitHub Actions | CI/CD automation |

---

# 4. Node.js Project Setup

The project was initialized as a Node.js application.

The initial Node.js project configuration was created using:

```bash
npm init -y
```

This command creates the `package.json` file.

The `package.json` file contains the project's metadata, scripts, dependencies, and development dependencies.

The project was then configured to use CommonJS:

```json
"type": "commonjs"
```

This allows the application to use the CommonJS module system.

---

# 5. Project Structure

The project structure is:

```text
nodejs-demo-app/
│
├── .github/
│   └── workflows/
│       └── main.yml
│
├── app.js
├── app.test.js
├── Dockerfile
├── .dockerignore
├── .gitignore
├── package.json
├── package-lock.json
├── README.md
└── PROJECT_DOCUMENTATION.md
```

## Purpose of Each File

| File / Folder | Purpose |
|---|---|
| `.github/workflows/main.yml` | GitHub Actions CI/CD workflow |
| `app.js` | Main Node.js application |
| `app.test.js` | Automated application tests |
| `Dockerfile` | Instructions for creating the Docker image |
| `.dockerignore` | Prevents unnecessary files from entering the Docker build context |
| `.gitignore` | Prevents unwanted files from being committed to Git |
| `package.json` | Node.js project configuration |
| `package-lock.json` | Locks dependency versions |
| `README.md` | Main project documentation |
| `PROJECT_DOCUMENTATION.md` | Complete technical documentation |

---

# 6. Application Dependencies

The project uses Express as the application dependency.

Testing uses:

- Jest
- Supertest

The important part of `package.json` is:

```json
{
  "scripts": {
    "start": "node app.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^5.2.1"
  },
  "devDependencies": {
    "jest": "^30.5.2",
    "supertest": "^7.3.0"
  },
  "type": "commonjs"
}
```

## Why Express?

Express provides the HTTP server functionality required by the Node.js web application.

## Why Jest?

Jest is used as the test runner. It discovers and executes the automated test cases.

## Why Supertest?

Supertest is used to test the HTTP application without requiring manual browser testing for the automated test cases.

---

# 7. package.json

The project uses the following package configuration:

```json
{
  "name": "nodejs-demo-app",
  "version": "1.0.0",
  "description": "Node.js application for DevOps CI/CD task",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "jest"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "commonjs",
  "dependencies": {
    "express": "^5.2.1"
  },
  "devDependencies": {
    "jest": "^30.5.2",
    "supertest": "^7.3.0"
  }
}
```

## Important fields

### `name`

```json
"name": "nodejs-demo-app"
```

Defines the project name.

### `version`

```json
"version": "1.0.0"
```

Defines the current project version.

### `main`

```json
"main": "app.js"
```

Specifies the main application entry file.

### `scripts`

```json
"scripts": {
  "start": "node app.js",
  "test": "jest"
}
```

The `start` script starts the application.

The `test` script executes the automated tests.

This is important for CI/CD because GitHub Actions can execute:

```bash
npm test
```

without manually specifying the testing command.

---

# 8. Installing Dependencies

Dependencies were installed using:

```bash
npm install
```

This installs the packages specified in `package.json` and creates or updates:

```text
package-lock.json
```

The lock file records the exact dependency resolution used by npm.

---

# 9. package-lock.json

`package-lock.json` is important for reproducible installations.

The Dockerfile uses:

```bash
npm ci
```

instead of:

```bash
npm install
```

`npm ci` is intended for clean and reproducible dependency installation when a lock file is available.

It also helps ensure that the dependency versions used by the local environment and automated build are consistent.

---

# 10. Local Dependency Verification

The project dependencies were verified locally.

The clean installation command used was:

```bash
npm ci
```

The npm registry connection was also checked using:

```bash
npm ping
```

The installation eventually completed successfully with no reported vulnerabilities.

---

# 11. Node.js Application

The main application file is:

```text
app.js
```

This file contains the Express application and starts the HTTP server.

The application runs on port:

```text
3000
```

The application can therefore be tested locally through the Node.js server.

The purpose of this application in the internship task is not to build a large business application. It provides a simple web application that can be tested, containerized, and included in the CI/CD pipeline.

---

# 12. Automated Testing

The automated test file is:

```text
app.test.js
```

The project uses:

```text
Jest
Supertest
```

The test command is:

```bash
npm test
```

The purpose of automated testing in this project is to verify the application before continuing with the CI/CD process.

Conceptually:

```text
Code
 |
 v
Run Tests
 |
 +---- FAIL ----> Stop pipeline
 |
 +---- PASS ----> Continue
```

This is important because a CI/CD pipeline should not automatically build and publish code that has failed its automated tests.

---

# 13. Docker Containerization

Docker is used to package the Node.js application and its runtime environment into a container image.

The project contains:

```text
Dockerfile
```

The Dockerfile defines how the image is created.

---

# 14. Dockerfile

The Dockerfile used for the project is:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# 15. Dockerfile Line-by-Line Explanation

## `FROM node:22-alpine`

```dockerfile
FROM node:22-alpine
```

This specifies the base image.

The image provides Node.js 22 on Alpine Linux.

The base image gives the container the Node.js runtime required to execute the application.

---

## `WORKDIR /app`

```dockerfile
WORKDIR /app
```

This sets `/app` as the working directory inside the container.

Subsequent commands operate relative to this directory.

Instead of placing application files in the container root directory, the application is organized inside:

```text
/app
```

---

## `COPY package*.json ./`

```dockerfile
COPY package*.json ./
```

This copies the package files from the project into the container.

The wildcard matches:

```text
package.json
package-lock.json
```

These files are copied before the application source code.

This allows Docker to cache the dependency installation layer when only application source files change.

---

## `RUN npm ci`

```dockerfile
RUN npm ci
```

This installs the project's dependencies inside the Docker image.

`npm ci` uses the dependency lock file and performs a clean installation.

This step is important because the resulting Docker image must contain the packages required by the application.

---

## `COPY . .`

```dockerfile
COPY . .
```

This copies the remaining project files into the container working directory.

At this stage the application source code becomes available inside:

```text
/app
```

---

## `EXPOSE 3000`

```dockerfile
EXPOSE 3000
```

This documents that the application inside the container listens on port 3000.

The application itself listens on port 3000.

`EXPOSE` does not publish the port to the host by itself. Port mapping is configured when the container is started.

---

## `CMD ["npm", "start"]`

```dockerfile
CMD ["npm", "start"]
```

This specifies the default command executed when the container starts.

It runs the `start` script from `package.json`:

```json
"start": "node app.js"
```

Therefore the container starts the Node.js application.

---

# 16. .dockerignore

The project also uses:

```text
.dockerignore
```

The purpose of `.dockerignore` is to prevent unnecessary files from being included in the Docker build context.

This can reduce the amount of data sent to Docker and prevent local-only files from being copied into the image.

Typical examples include:

```text
node_modules
npm-debug.log
.git
.gitignore
```

The exact contents should match the `.dockerignore` file committed in the repository.

---

# 17. Building the Docker Image

The Docker image was built using:

```bash
docker build -t nodejs-demo-app .
```

Explanation:

### `docker build`

Creates a Docker image from the Dockerfile.

### `-t nodejs-demo-app`

Assigns the image name:

```text
nodejs-demo-app
```

### `.`

Uses the current directory as the Docker build context.

The successful build produced:

```text
nodejs-demo-app:latest
```

---

# 18. Docker Build Problem: Docker Engine

Initially, Docker commands could not build the image because the Docker Desktop engine was not running correctly.

The Docker environment was checked and Docker Desktop was started.

The Docker installation was then verified using:

```bash
docker run --rm hello-world
```

The command successfully produced Docker's:

```text
Hello from Docker!
```

message.

This confirmed that the Docker engine was functioning.

---

# 19. Docker Build Problem: package-lock Synchronization

During an earlier Docker build, `npm ci` reported that `package.json` and `package-lock.json` were not synchronized.

The error included missing packages from the lock file.

The lock file was regenerated using:

```bash
docker run --rm -v "${PWD}:/app" -w /app node:22-alpine npm install --package-lock-only
```

After the lock file was synchronized, the Docker image build completed successfully.

This demonstrates why `package.json` and `package-lock.json` must remain consistent when using:

```bash
npm ci
```

---

# 20. Docker Build Problem: npm Error

During one build attempt, npm reported:

```text
npm error Exit handler never called!
```

The build was retried and subsequently completed successfully.

The final Docker build reached the image creation stage and produced:

```text
nodejs-demo-app:latest
```

---

# 21. Running the Docker Container

The first attempt used host port 3000:

```bash
docker run --rm -p 3000:3000 nodejs-demo-app
```

Port 3000 was already being used by another Node.js process.

The port was checked using:

```powershell
netstat -ano | findstr :3000
```

The process was identified as:

```text
node.exe
```

A different host port was therefore used.

The successful command was:

```bash
docker run --rm -p 3002:3000 nodejs-demo-app
```

This means:

```text
Host port 3002
       |
       v
Container port 3000
       |
       v
Node.js application
```

The container successfully started and displayed:

```text
Server running on port 3000
```

This verified that the Dockerized application was working.

---

# 22. Host Port vs Container Port

The command:

```bash
docker run --rm -p 3002:3000 nodejs-demo-app
```

uses the format:

```text
HOST_PORT:CONTAINER_PORT
```

Therefore:

```text
3002:3000
```

means:

- Port `3002` is used on the host machine.
- Port `3000` is used inside the container.

The application continues to listen on port 3000 inside the container.

---

# 23. Git Initialization and Version Control

Git is used to track project changes.

The project was maintained as a Git repository and connected to GitHub.

The purpose of Git is to provide version control.

The purpose of GitHub is to host the repository remotely and provide the source-code location that triggers GitHub Actions.

---

# 24. GitHub Repository

A separate GitHub repository was created for this internship task.

The repository contains the application source code and the CI/CD configuration.

The repository structure includes:

```text
.github/
└── workflows/
    └── main.yml
```

The workflow file is therefore located at:

```text
.github/workflows/main.yml
```

---

# 25. Why a Separate Repository Was Used

A separate repository was used for this internship task so that the task can be submitted independently.

This keeps the task's:

- application code
- Docker configuration
- GitHub Actions workflow
- documentation

together in one repository.

---

# 26. GitHub Actions CI/CD

GitHub Actions is used to automate the CI/CD process.

The workflow file is:

```text
.github/workflows/main.yml
```

GitHub Actions reads YAML workflow files from the `.github/workflows` directory.

The general CI/CD process for this project is:

```text
Push code to main
        |
        v
GitHub Actions starts
        |
        v
Install dependencies
        |
        v
Run automated tests
        |
        v
Build Docker image
        |
        v
Login to Docker Hub
        |
        v
Push Docker image
```

---

# 27. Why CI/CD Is Used

Without CI/CD, the developer would have to manually perform the following operations after changes:

```text
Install dependencies
        ↓
Run tests
        ↓
Build Docker image
        ↓
Login to Docker Hub
        ↓
Push Docker image
```

GitHub Actions automates these operations.

This improves consistency and reduces repetitive manual work.

---

# 28. Workflow Trigger

The internship task requires the workflow to run when code is pushed to the main branch.

The expected trigger concept is:

```yaml
on:
  push:
    branches:
      - main
```

This means that a push to the `main` branch can start the workflow.

The exact trigger configuration must match the `main.yml` committed in the repository.

---

# 29. GitHub Actions Workflow Concepts

A GitHub Actions workflow is normally organized into:

```text
Workflow
   |
   +---- Jobs
          |
          +---- Steps
```

## Workflow

The complete automation definition stored in:

```text
.github/workflows/main.yml
```

## Job

A group of related steps executed by a GitHub Actions runner.

Examples include:

```text
test
build
push
```

## Step

An individual command or reusable GitHub Action executed within a job.

Examples:

```text
checkout source code
install Node.js
run npm ci
run npm test
build Docker image
push Docker image
```

---

# 30. GitHub Actions Runner

A runner is the machine/environment that executes the workflow.

A common GitHub-hosted runner is:

```text
ubuntu-latest
```

The runner provides the environment in which commands such as:

```bash
npm ci
npm test
docker build
```

can be executed.

The exact runner configured in the project's `main.yml` should be treated as the source of truth.

---

# 31. GitHub Actions Secrets

Docker Hub credentials were added to GitHub repository Secrets.

The credentials are intended to be referenced from the workflow rather than written directly into the YAML file.

This prevents credentials from being hard-coded into the source code.

The workflow should access secrets through GitHub's secrets mechanism.

For example, secret values can be referenced using the GitHub Actions expression format:

```text
${{ secrets.SECRET_NAME }}
```

The actual secret names used in the repository should match the names configured in GitHub.

---

# 32. Why Secrets Are Required

Docker Hub authentication requires credentials.

Credentials should not be written directly into:

```text
main.yml
```

because the workflow file is stored in the Git repository.

Instead, credentials are stored as GitHub repository secrets.

Conceptually:

```text
GitHub Repository
       |
       +---- main.yml
       |
       +---- GitHub Secrets
                 |
                 +---- Docker credentials
```

The workflow accesses the credentials during execution.

---

# 33. Docker Hub

Docker Hub is used as the Docker image registry.

The purpose of Docker Hub in this project is to store the Docker image produced by the CI/CD pipeline.

The general process is:

```text
Application Source
       |
       v
Docker Build
       |
       v
Docker Image
       |
       v
Docker Hub
```

Once pushed to Docker Hub, the image can be pulled from the registry by an environment that has access to it.

---

# 34. Docker Image Naming

The local image built during development was:

```text
nodejs-demo-app:latest
```

When publishing to Docker Hub, the image normally needs to be associated with the Docker Hub namespace.

The general format is:

```text
DOCKER_USERNAME/IMAGE_NAME:TAG
```

For example:

```text
username/nodejs-demo-app:latest
```

The actual Docker Hub username and repository name should match the Docker Hub repository configured for the project.

---

# 35. CI/CD Quality Gate

Testing should happen before publishing the Docker image.

The intended sequence is:

```text
Push
 |
 v
Install dependencies
 |
 v
Run tests
 |
 +---- Tests fail
 |       |
 |       v
 |    Stop workflow
 |
 +---- Tests pass
         |
         v
      Build image
         |
         v
      Push image
```

This prevents the pipeline from publishing an image when the automated tests fail.

---

# 36. Complete Project Flow

The complete project can be understood as five stages.

## Stage 1 – Development

The application is written using Node.js and Express.

```text
app.js
```

contains the application.

---

## Stage 2 – Testing

Automated tests are created using:

```text
Jest
Supertest
```

They are executed using:

```bash
npm test
```

---

## Stage 3 – Containerization

Docker packages the application.

The Dockerfile:

```dockerfile
FROM node:22-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

creates the Docker image.

---

## Stage 4 – Continuous Integration

GitHub Actions automatically:

```text
checks out the code
        ↓
installs dependencies
        ↓
runs tests
```

---

## Stage 5 – Continuous Delivery

After the required checks succeed:

```text
Docker image
      ↓
Docker Hub
```

The image is pushed to the configured Docker Hub repository.

---

# 37. Problems Faced and Solutions

## Problem 1 – Docker Engine Not Running

### Problem

Docker build initially failed because the Docker engine was unavailable.

### Solution

Docker Desktop was started and Docker functionality was verified using:

```bash
docker run --rm hello-world
```

---

## Problem 2 – package.json and package-lock.json Out of Sync

### Problem

`npm ci` reported missing packages from the lock file.

### Solution

The lock file was regenerated using:

```bash
docker run --rm -v "${PWD}:/app" -w /app node:22-alpine npm install --package-lock-only
```

After synchronization, the Docker build succeeded.

---

## Problem 3 – Docker Port 3000 Already in Use

### Problem

Running:

```bash
docker run --rm -p 3000:3000 nodejs-demo-app
```

failed because host port 3000 was already occupied.

### Investigation

The port was checked using:

```powershell
netstat -ano | findstr :3000
```

The process was identified as `node.exe`.

### Solution

Another available host port was used:

```bash
docker run --rm -p 3002:3000 nodejs-demo-app
```

The container then started successfully.

---

## Problem 4 – npm Error During Docker Build

### Problem

One Docker build attempt reported:

```text
npm error Exit handler never called!
```

### Solution

The build was retried.

The subsequent build completed successfully and produced:

```text
nodejs-demo-app:latest
```

---

# 38. Verification Checklist

The project was verified through the following stages:

### Node.js

```bash
npm install
```

Dependency installation completed.

### Clean npm installation

```bash
npm ci
```

The clean installation completed successfully.

### npm connectivity

```bash
npm ping
```

The npm registry connection was checked.

### Docker engine

```bash
docker run --rm hello-world
```

Docker successfully ran the test container.

### Docker image

```bash
docker build -t nodejs-demo-app .
```

The Docker image was successfully built.

### Docker container

```bash
docker run --rm -p 3002:3000 nodejs-demo-app
```

The Node.js application successfully started inside the Docker container.

### GitHub

The project was placed in a separate GitHub repository for the internship task.

### GitHub Actions

The CI/CD workflow is stored under:

```text
.github/workflows/main.yml
```

---

# 39. Important Commands Used

## Initialize Node.js project

```bash
npm init -y
```

## Install dependencies

```bash
npm install
```

## Clean dependency installation

```bash
npm ci
```

## Test the application

```bash
npm test
```

## Check npm registry connectivity

```bash
npm ping
```

## Build Docker image

```bash
docker build -t nodejs-demo-app .
```

## Run Docker container

```bash
docker run --rm -p 3002:3000 nodejs-demo-app
```

## Verify Docker installation

```bash
docker run --rm hello-world
```

## Check port usage on Windows

```powershell
netstat -ano | findstr :3000
```

---

# 40. CI/CD Terminology

## Continuous Integration (CI)

Continuous Integration means automatically integrating and validating code changes.

In this project, testing is part of the CI process.

```text
Code Push
   ↓
Install
   ↓
Test
```

---

## Continuous Delivery / Deployment

Continuous Delivery/Deployment automates the process of preparing and delivering application artifacts.

In this project, the Docker image is built and intended to be published to Docker Hub.

```text
Test
 ↓
Docker Build
 ↓
Docker Hub
```

---

## Pipeline

A pipeline is the complete automated sequence.

For this project:

```text
Code
 ↓
Test
 ↓
Build
 ↓
Push
```

---

## Runner

A runner is the machine/environment that executes GitHub Actions jobs.

---

## Job

A job is a group of related workflow steps.

---

## Step

A step is an individual action or command inside a job.

---

## Secret

A secret is protected information such as a password or access token.

Secrets should not be hard-coded into source files.

---

# 41. Why Docker Is Used

Docker provides a consistent environment for running the application.

Without containerization, the application depends directly on the host environment.

With Docker:

```text
Application
+
Node.js runtime
+
Dependencies
+
Configuration
        |
        v
Docker Image
```

The same image can then be used in different environments.

---

# 42. Why Docker Hub Is Used

Docker Hub provides a registry where Docker images can be stored.

Instead of keeping the image only on the developer's computer:

```text
Local Docker Image
```

the CI/CD pipeline can publish it to:

```text
Docker Hub
```

This makes the image available to environments that need to pull it.

---

# 43. Why GitHub Actions Is Used

GitHub Actions integrates directly with the GitHub repository.

A push to the repository can automatically start the workflow.

This removes the need to manually repeat:

```text
test
build
push
```

after every code change.

---

# 44. Why `npm ci` Is Used in CI/CD

`npm ci` is suitable for automated environments because it installs dependencies from the lock file.

The objective is reproducibility.

The same dependency versions represented by the lock file can therefore be installed in the automated environment.

---

# 45. Why Testing Comes Before Docker Image Publishing

Testing before publishing provides a quality gate.

If tests fail:

```text
Tests
  |
  X
Failure
  |
  v
Stop
```

If tests pass:

```text
Tests
  |
  ✓
Pass
  |
  v
Docker Build
  |
  v
Docker Push
```

This reduces the chance of publishing an image from code that has failed its automated tests.

---

# 46. Interview Questions and Answers

## Q1. What is CI/CD?

CI/CD is a software development practice that automates processes such as code integration, testing, building, and delivery.

---

## Q2. What is GitHub Actions?

GitHub Actions is GitHub's automation platform used to execute workflows in response to repository events.

---

## Q3. What is a GitHub Actions runner?

A runner is the environment that executes the commands defined by a GitHub Actions job.

---

## Q4. What is the difference between a job and a step?

A job is a group of steps.

A step is an individual command or action executed as part of the job.

---

## Q5. Why are GitHub Secrets used?

Secrets are used to securely store sensitive values such as Docker credentials instead of putting them directly into the workflow file.

---

## Q6. Why should passwords not be written directly in `main.yml`?

Because `main.yml` is part of the source repository and sensitive credentials could become exposed.

GitHub Secrets provide a safer mechanism for supplying credentials to workflows.

---

## Q7. What happens if a CI test fails?

The pipeline should stop before later stages that depend on successful testing, such as publishing the Docker image.

---

## Q8. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

---

## Q9. What does `FROM` do in a Dockerfile?

`FROM` specifies the base image used to create the new image.

---

## Q10. What does `WORKDIR` do?

`WORKDIR` sets the working directory inside the container.

---

## Q11. What does `EXPOSE 3000` mean?

It documents that the application uses port 3000 inside the container.

It does not itself publish the port to the host.

---

## Q12. What does `CMD ["npm", "start"]` do?

It starts the application using the `start` script defined in `package.json`.

---

## Q13. Why use Docker?

Docker packages the application and its runtime dependencies into a portable container image.

---

## Q14. Why use Docker Hub?

Docker Hub provides a registry for storing and distributing Docker images.

---

## Q15. What is the purpose of `package-lock.json`?

It records the dependency resolution used by npm and supports reproducible installations.

---

# 47. Final Project Architecture

```text
                    Developer
                        |
                        | git push
                        v
                +----------------+
                |    GitHub      |
                |  Repository    |
                +----------------+
                        |
                        v
                +----------------+
                | GitHub Actions |
                +----------------+
                        |
                        v
                 Install packages
                        |
                        v
                    Run tests
                        |
                  +-----+-----+
                  |           |
                FAIL         PASS
                  |           |
                  |           v
                  |      Docker Build
                  |           |
                  |           v
                  |      Docker Image
                  |           |
                  |           v
                  |      Docker Hub
                  |           
                  v
                 Stop
```

---

# 48. Final Result

The project demonstrates the main components required for the internship task:

```text
Node.js Application
        ↓
Automated Tests
        ↓
Docker Containerization
        ↓
GitHub Repository
        ↓
GitHub Actions CI/CD
        ↓
Docker Hub Image Publishing
```

The local Node.js application was successfully installed and tested.

The Docker environment was successfully verified.

The Docker image:

```text
nodejs-demo-app:latest
```

was successfully built.

The Dockerized application was successfully started using:

```bash
docker run --rm -p 3002:3000 nodejs-demo-app
```

The GitHub Actions workflow is stored at:

```text
.github/workflows/main.yml
```

The project documentation is maintained in this single file:

```text
PROJECT_DOCUMENTATION.md
```

---

# 49. Repository Submission Checklist

Before submitting the internship task, verify that the GitHub repository contains:

```text
nodejs-demo-app/
│
├── .github/
│   └── workflows/
│       └── main.yml
│
├── app.js
├── app.test.js
├── Dockerfile
├── .dockerignore
├── .gitignore
├── package.json
├── package-lock.json
├── README.md
└── PROJECT_DOCUMENTATION.md
```

Also verify:

- The repository is accessible to the evaluator according to the internship submission requirements.
- The workflow file is present under `.github/workflows/`.
- Docker Hub credentials are stored as GitHub Secrets rather than hard-coded.
- The application can be installed.
- The tests can be executed.
- The Docker image can be built.
- The Docker container can start.
- The repository contains the required documentation and screenshots if required by the internship submission instructions.

---

# 50. Important Note About `main.yml`

The exact contents of:

```text
.github/workflows/main.yml
```

must be documented from the actual workflow file stored in the repository.

The workflow should not be replaced with an invented example in project documentation.

For an exact line-by-line explanation, use the committed `main.yml` as the source of truth.

This keeps the documentation accurate and prevents the documentation from claiming that a workflow contains commands that are not actually present.

---

# 51. Conclusion

This project demonstrates a basic DevOps CI/CD pipeline for a Node.js application.

The application is tested automatically, packaged using Docker, and prepared for image publishing through GitHub Actions and Docker Hub.

The main DevOps flow is:

```text
Develop
   ↓
Commit
   ↓
Push to GitHub
   ↓
GitHub Actions
   ↓
Install Dependencies
   ↓
Run Tests
   ↓
Build Docker Image
   ↓
Push Image to Docker Hub
```

The project therefore combines source control, automated testing, containerization, and CI/CD automation into one workflow.
