# My Git-Powered DevOps Project

Welcome to my project! The goal here was simple: build a clean, real-world DevOps project from scratch while following strict **Git best practices**. 

I set up a secure multi-branch system, containerized a small test app with Docker, managed everything via Pull Requests, and locked in a final production release using Git tags.

## 🛠️ How the Branching Works (The Strategy)

Instead of pushing directly to production and hoping for the best, I used a structured approach to keep the code safe:
* **`main`**: My production-ready line. Only clean, tested, and fully approved code lives here.
* **`dev`**: The staging ground. This is where features gather to get tested together before going live.
* **`feature/setup-dockerfile`**: My isolated sandbox. I built and tested my files here without messing up anyone else's workspace.

---

## 🚶‍♂️ Walkthrough: How I Built It

### 1. Starting Fresh (and Fixing Folder Mix-ups)
I initially initialized Git inside my root user folder by mistake, which tried tracking all my personal Windows files (`AppData`, system secrets, etc.)! To fix that, I cleaned out the tracking, moved to a dedicated directory, and started completely fresh:
```bash
# Set up a dedicated workspace
cd ~/Desktop
mkdir vc-devops-project
cd vc-devops-project

# Started a clean Git repo and linked it to GitHub
git init
git branch -M main
git remote add origin https://github.com
```

### 2. Setting Up Boundaries (`.gitignore`)
To make sure secret keys, local logs, and system junk didn't slip into GitHub, I created a `.gitignore` file to filter out the noise:
```text
*.log
.env
build/
.DS_Store
```

### 3. Writing the Feature (The Dockerfile)
I jumped onto the `dev` branch, spun off a custom `feature` branch, and opened Notepad to write a quick, lightweight script:
```bash
git checkout -b dev
git push origin dev
git checkout -b feature/setup-dockerfile
```

Inside my `Dockerfile`, I added a minimal image that just says hello:
```dockerfile
FROM alpine:latest
RUN apk update && apk add --no-cache curl
CMD ["echo", "Hello from my DevOps Project!"]
```

Then I safely committed and pushed my feature branch up to GitHub:
```bash
git add .
git commit -m "feat: add initial Dockerfile for application containerization"
git push origin feature/setup-dockerfile
```

### 4. Code Reviews & The Pull Request
Instead of forcing the code into `dev` locally, I opened a **Pull Request (PR)** on GitHub. I pointed the branch to merge into `dev`, reviewed the code cleanly on the web interface, and successfully clicked **"Merge pull request"**.

### 5. Going Live & Tagging the Release
Once the feature was safe in staging, it was time to push it to the live production branch (`main`) and give it an official version label:
```bash
# Sync my local machine with the GitHub web merge
git checkout dev
git pull origin dev

# Merge the staging updates into the main production line
git checkout main
git merge dev
git push origin main

# Mark our milestone with a permanent version tag
git tag -a v1.0.0 -m "release: stable version 1.0.0"
git push origin v1.0.0
```

---

## 🐳 Running it Locally

If you want to pull this down and run my container application yourself, run these two quick commands:

```bash
# 1. Build the container image
docker build -t devops-app .

# 2. Run the application
docker run --rm devops-app
```

**What you should see in your terminal:**
```text
Hello from my DevOps Project!
```
