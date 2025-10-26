
# 🚀 My N8N Setup - Workflow Automation Made Easy

A simple and efficient Docker-based setup for N8N, a powerful workflow automation tool. Get up and running in minutes!

![Docker](https://img.shields.io/badge/Docker-Enabled-blue?logo=docker)
![N8N](https://img.shields.io/badge/N8N-Automation-orange?logo=n8n)
![Easy Setup](https://img.shields.io/badge/Setup-5__Minutes-green)

## 📋 Prerequisites

Before you begin, ensure you have:
- **Docker Desktop** installed on your system
- **Visual Studio Code** (or any code editor)
- Basic terminal/command line knowledge

## 🛠 Quick Setup Guide

### Step 1: Install and Launch Docker
- Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Launch Docker Desktop and wait for it to be fully running

### Step 2: Pull N8N Image
- Open Docker Desktop
- Go to **Images** tab
- Search for `n8nio/n8n`
- Click **Pull** to download the official N8N image

### Step 3: Project Setup
```bash
# Create project directory (no spaces in name!)
mkdir docker-n8n
cd docker-n8n
```

### Step 4: Docker Compose Configuration
Create a `docker-compose.yml` file in your project directory:

```yaml
services:
  n8n:
    image: n8nio/n8n:latest 
    container_name: n8n-vscode
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_EDITOR_BASE_URL=http://localhost:5678
      - N8N_WEBHOOK_URL=http://localhost:5678/
      - N8N_SECURE_COOKIE=false
      - N8N_DEFAULT_BINARY_DATA_MODE=filesystem
      - N8N_COMMUNITY_PACKAGES_ALLOW_TOOL_USAGE=true
      - N8N_RUNNERS_ENABLED=true
      - N8N_LOG_LEVEL=debug
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
      - EXECUTIONS_PROCESS=main
      - EXECUTIONS_DATA_SAVE_ON_ERROR=all
      - EXECUTIONS_DATA_SAVE_ON_SUCCESS=none
      - N8N_METRICS=true
    # Timezone settings for Bangladesh
      - TZ=Asia/Dhaka
      - GENERIC_TIMEZONE=Asia/Dhaka
    volumes:
      - n8n_storage:/home/node/.n8n
      - ./shared-data:/shared
      - /mnt/data:/mnt/data

volumes:
  n8n_storage:
    driver: local
```

### Then, Dockerfile Configuration
Create a `Dockerfile` file in your project directory:

```yaml
FROM n8nio/n8n:latest

# Install Python3, pip, curl, and yt-dlp
USER root
RUN apk add --update python3 py3-pip curl yt-dlp nano bash

# Install pipx for Node user
USER node
RUN python3 -m pip install --user --break-system-packages pipx

# Add pipx binary path to PATH
ENV PATH="/home/node/.local/bin:$PATH"
```
# add more apk name here : USER root
**RUN apk add --no-cache python3 py3-pip curl yt-dlp nano**

### Build 
```
docker build -t n8n-vscode .
```

### Now Restart Docker 
```
docker compose down
```
### Update  `docker-compose.yml` file in your project directory:

```yaml
services:
  n8n:
    image: n8n-vscode 
    container_name: n8n-vscode
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_EDITOR_BASE_URL=http://localhost:5678
      - N8N_WEBHOOK_URL=http://localhost:5678/
      - N8N_SECURE_COOKIE=false
      - N8N_DEFAULT_BINARY_DATA_MODE=filesystem
      - N8N_COMMUNITY_PACKAGES_ALLOW_TOOL_USAGE=true
      - N8N_RUNNERS_ENABLED=true
      - N8N_LOG_LEVEL=debug
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
      - EXECUTIONS_PROCESS=main
      - EXECUTIONS_DATA_SAVE_ON_ERROR=all
      - EXECUTIONS_DATA_SAVE_ON_SUCCESS=none
      - N8N_METRICS=true
    # Timezone settings for Bangladesh
      - TZ=Asia/Dhaka
      - GENERIC_TIMEZONE=Asia/Dhaka
    volumes:
      - n8n_storage:/home/node/.n8n
      - ./shared-data:/shared
      - /mnt/data:/mnt/data

volumes:
  n8n_storage:
    driver: local
```

### Step 5: Launch N8N
Open VS Code terminal in your project directory and run:

```bash
docker compose up -d
```

### Step 6: Access N8N
- Wait approximately **2 minutes** for initialization
- Open your browser and navigate to:
```
http://localhost:5678/
```

## 🔧 Configuration Options

### Environment Variables (Optional)
Modify the `docker-compose.yml` to customize:

```yaml
environment:
  - N8N_BASIC_AUTH_ACTIVE=true
  - N8N_BASIC_AUTH_USER=your_username
  - N8N_BASIC_AUTH_PASSWORD=your_secure_password
  - N8N_HOST=localhost
```

### Port Configuration
Change the first port number to use a different local port:
```yaml
ports:
  - "8080:5678"  # Now accessible at http://localhost:8080
```

## 📁 Project Structure
```
docker-n8n/
├── docker-compose.yml
└── Dockerfile

```

## 🚀 What is N8N?

N8N is a fair-code licensed workflow automation tool that helps you:
- 🤖 **Automate tasks** across different services
- 🔄 **Connect APIs** and services seamlessly
- 📊 **Create complex workflows** with a visual editor
- 💾 **Self-host** your automation platform

## 🛑 Stopping the Service

To stop N8N when not in use:
```bash
docker compose down
```

## 🔄 Restarting N8N

To restart the service:
```bash
docker compose restart
```

## ❓ Troubleshooting

### Common Issues:
1. **Port already in use**: Change the first port in `docker-compose.yml`
2. **Docker not running**: Ensure Docker Desktop is active
3. **Access issues**: Wait 2+ minutes for full initialization
4. **Permission errors**: Run terminal as administrator if needed

### Check Service Status:
```bash
docker ps
docker logs docker-n8n-n8n-1
```

## 📚 Useful Links

- [N8N Official Documentation](https://docs.n8n.io/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [N8N Community Forum](https://community.n8n.io/)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

---

**⭐ Star this repo if you found it helpful!**

*Happy automating! 🎉*

## How to see your configuration in docker

https://github.com/user-attachments/assets/fdcb2961-e03e-4977-83f5-eb0df9ca6290




# n8n Localhost URL Issue and Resolution

## Problem Overview

When setting up **n8n** with Docker, you may encounter the following situation:

* `http://127.0.0.1:5678/` works perfectly.
* `http://localhost:5678/` does **not** work.

<img width="1919" height="950" alt="Screenshot 2025-10-26 235207" src="https://github.com/user-attachments/assets/e3c7b34c-ce4b-4b1c-8425-43ab723e7955" />
<img width="1919" height="1007" alt="Screenshot 2025-10-26 221509" src="https://github.com/user-attachments/assets/c4ec9098-c38b-46d4-82d5-4bf767fb3371" />
<img width="1797" height="1015" alt="Screenshot 2025-10-26 202004" src="https://github.com/user-attachments/assets/b786e5c3-c669-4e32-98fc-56fce2d431c3" />
<img width="987" height="222" alt="Screenshot 2025-10-26 201557" src="https://github.com/user-attachments/assets/581f9359-ceb3-4ddc-a2c1-c01d2564f4f1" />
<img width="1460" height="914" alt="Screenshot 2025-10-26 201154" src="https://github.com/user-attachments/assets/035a5fdc-a8ef-454a-b556-0bcc8c19bec1" />


This can cause problems in scenarios where certain integrations or APIs (e.g., Google APIs) require exact URL matches for authorization. If the URL does not match the expected authorization URL, the API calls will fail.

---

## Cause

The problem usually occurs due to **hostname resolution issues** on your system. In Windows, the `localhost` mapping may not resolve correctly if there are conflicting entries in the `hosts` file or if Docker modifies network behavior.

For example, a problematic `hosts` file may look like this:

```
# localhost name resolution is handled within DNS itself.
# 127.0.0.1       localhost
# ::1             localhost
```

In some cases, restarting the PC resolves the issue because it refreshes network and DNS settings.

---

## Step-by-Step Solution

1. **Check the `hosts` file**

   On Windows, open:

   ```
   %SystemRoot%\System32\drivers\etc\hosts
   ```

   Ensure that the following entry exists and is **uncommented**:

   ```
   127.0.0.1       localhost
   ```

2. **Restart your PC**

   After updating the `hosts` file, restart your computer to apply changes.

3. **Verify**

   * Open a browser and visit:

     ```
     http://localhost:5678/
     ```
   * It should now work correctly, alongside:

     ```
     http://127.0.0.1:5678/
     ```

---

## Previous Setup vs New Fix

**Previous Setup:**

```yaml
environment:
  - N8N_EDITOR_BASE_URL=http://localhost:5678
  - N8N_WEBHOOK_URL=http://localhost:5678/
```

**Problem:** `localhost` URL does not resolve correctly.

**New Fix:**

* Ensure `localhost` resolves in `hosts` file.
* Restart PC to refresh network configuration.
* Now `localhost` works, and all integrations (including Google APIs) function properly.

---

## Why This Matters

Many integrations require that the **authorization URL exactly matches** what is configured in the API console. If `localhost` is not properly resolving:

* OAuth flows will fail.
* API requests may be rejected.

With this fix, users can reliably use `localhost` in their environment without switching to `127.0.0.1`.

---

## TL;DR

* Issue: `http://localhost:5678/` not working, only `127.0.0.1` works.
* Cause: Hosts file or DNS resolution problem on Windows.
* Solution: Edit `hosts` file → ensure `127.0.0.1 localhost` → restart PC.

---




