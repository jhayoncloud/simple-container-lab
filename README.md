# Simple Container Lab

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js%2018-339933?style=flat&logo=node.js&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)

A small hands-on lab covering the basics of containerization with Docker and version control with Git. A Node.js application was written, packaged into a Docker image, run inside a container, and tracked through Git from the first commit to a GitHub push.

## Table of Contents

- [Simple Container Lab](#simple-container-lab)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Objectives](#objectives)
  - [Tech Stack](#tech-stack)
  - [Prerequisites](#prerequisites)
  - [Project Structure](#project-structure)
  - [Walkthrough](#walkthrough)
    - [1. Project Setup](#1-project-setup)
    - [2. Application Code](#2-application-code)
    - [3. Dockerfile](#3-dockerfile)
    - [4. Build the Image](#4-build-the-image)
    - [5. Verify the Image](#5-verify-the-image)
    - [6. Run the Container](#6-run-the-container)
    - [7. Git Initialization](#7-git-initialization)
    - [8. Staging and Commit](#8-staging-and-commit)
    - [9. Connect and Push to GitHub](#9-connect-and-push-to-github)
  - [Key Takeaways](#key-takeaways)

## Overview

This project demonstrates the end-to-end workflow of taking an application from source code to a version-controlled, containerized artifact. The focus is on the mechanics of Docker and Git, not the complexity of the code itself.

## Objectives

- Write a minimal Node.js application
- Define a Docker image build with a Dockerfile
- Build and tag a Docker image
- Run the application inside a container
- Distinguish between a Docker image and a running container
- Initialize a Git repository and create a clean commit history
- Connect a local repository to GitHub and push it

## Tech Stack

| Tool | Purpose |
| --- | --- |
| Docker | Building and running the containerized application |
| Node.js 18 (Alpine) | Runtime for the application, chosen for its small image footprint |
| Git | Local version control |
| GitHub | Remote repository hosting |

## Prerequisites

- Docker installed and running (`docker --version` to confirm)
- Git installed and configured with a GitHub account
- A terminal (this lab was done in VS Code's integrated terminal)

## Project Structure

```bash
container-lab/
├── app.js         # Node.js application
├── Dockerfile     # Image build instructions
└── README.md      # Project documentation
```

## Walkthrough

### 1. Project Setup

```bash
mkdir -p ~/container-lab && cd ~/container-lab
```

This command creates a dedicated directory for the lab and moves into it, keeping the Docker build context isolated from unrelated files on the machine.

### 2. Application Code

```bash
touch app.js && echo 'console.log("Hello, DevOps!");' > app.js
```

Creates `app.js`, the file that gets copied into the image and executed as the container's entry point.

### 3. Dockerfile

```bash
touch Dockerfile && echo -e 'FROM node:18-alpine\nCOPY app.js .\nCMD ["node","app.js"]' > Dockerfile
```

```dockerfile
FROM node:18-alpine
COPY app.js .
CMD ["node", "app.js"]
```

| Instruction | Purpose |
| --- | --- |
| `FROM` | Sets the base image — Node 18 on Alpine Linux for a smaller footprint |
| `COPY` | Copies `app.js` from the build context into the image |
| `CMD` | Defines the default command executed when a container starts |

### 4. Build the Image

```bash
docker build -t simple-container-lab:1.0 .
```

Reads the Dockerfile and builds a versioned image. The "`-t`" tags it as `simple-container-lab:1.0`; The "`.`" sets the current directory as the build context.

### 5. Verify the Image

```bash
docker image ls
```

Lists local images to confirm the build completed and produced `simple-container-lab:1.0` before running it.

### 6. Run the Container

```bash
docker run --rm simple-container-lab:1.0
```

Starts a container from the image and removes it automatically on exit. `--rm` keeps the local environment free of stopped containers left over from testing.

**Image vs. container:** the image is the static, versioned template; the container is a running instance of that template.

### 7. Git Initialization

```bash
git init
```

Initializes a local Git repository (`.git` directory) so the project's history can be tracked from this point forward.

### 8. Staging and Commit

```bash
git add .
git commit -m "feat: first container"
```

Stages all project files and commits them as the first snapshot in the repository's history — a checkpoint to diff against or roll back to later.

### 9. Connect and Push to GitHub

```bash
git remote add origin https://github.com/jhayoncloud/simple-container-lab.git
git branch -M main
git push -u origin main
```

Links the local repository to GitHub under the `origin` remote, renames the default branch to `main`, and pushes the commit history. `-u` sets the upstream so future pushes only require `git push`.

## Key Takeaways

- How a Dockerfile translates into a runnable, versioned image
- The practical difference between a Docker image and a container
- How to structure a clean Git history starting from the first commit
- The full local-to-remote workflow of linking and pushing to GitHub

This lab covers the core Docker and Git mechanics used throughout cloud and DevOps workflows — reproducible builds, version control, and remote collaboration.
