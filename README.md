
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
└── README.md
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
