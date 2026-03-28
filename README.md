
# Deploy FastAPI on a Fresh Ubuntu Server (GitHub + Docker)

This guide shows how to prepare a **clean Ubuntu server from scratch**, install **Git + Docker**, clone your **FastAPI project from GitHub**, and run it using **Docker Compose**.

Requirements:
- Fresh Ubuntu Server (20.04 / 22.04 / 24.04)
- A FastAPI project on GitHub
- `Dockerfile` + `docker-compose.yml` inside your repo

---

# 1. Update the Server

After connecting to your server, update all packages:

```bash
sudo apt update
sudo apt upgrade -y
````

---

# 2. Install Basic Required Tools

These packages are needed for installing Docker and working with the server:

```bash
sudo apt install -y curl wget nano unzip ca-certificates gnupg lsb-release
```

---

# 3. Install Git

Git is needed to pull your FastAPI code from GitHub:

```bash
sudo apt install -y git
```

Check Git version:

```bash
git --version
```

---

# 4. Install Docker (Official Method)

First, remove old Docker versions if they exist:

```bash
sudo apt remove -y docker docker-engine docker.io containerd runc
```

---

## 4.1 Add Docker GPG Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

---

## 4.2 Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## 4.3 Update Packages Again

```bash
sudo apt update
```

---

## 4.4 Install Docker Engine + Docker Compose Plugin

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Check Docker installation:

```bash
docker --version
docker compose version
```

---

# 5. Enable Docker Service

Start Docker and make it auto-start after reboot:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Check Docker status:

```bash
sudo systemctl status docker
```

---

# 6. Run Docker Without sudo (Recommended)

To avoid writing `sudo` every time:

```bash
sudo usermod -aG docker $USER
```

Apply group changes immediately:

```bash
newgrp docker
```

Test Docker:

```bash
docker run hello-world
```

---

# 7. Clone Your FastAPI Project from GitHub

Go to the recommended deployment directory:

```bash
cd /opt
```

Clone your repository (replace USERNAME and REPO_NAME):

```bash
sudo git clone https://github.com/USERNAME/REPO_NAME.git
```

Give ownership to your current user:

```bash
sudo chown -R $USER:$USER /opt/REPO_NAME
```

Enter the project folder:

```bash
cd /opt/REPO_NAME
```

---

# 8. Run FastAPI Using Docker Compose

Build and start the containers:

```bash
docker compose up -d --build
```

Check running containers:

```bash
docker ps
```

View logs:

```bash
docker compose logs -f
```

---

# 9. Firewall Setup (Optional but Recommended)

Install firewall:

```bash
sudo apt install -y ufw
```

Allow SSH access (IMPORTANT):

```bash
sudo ufw allow OpenSSH
```

Allow FastAPI port (example: 8000):

```bash
sudo ufw allow 8000
```

Enable firewall:

```bash
sudo ufw enable
```

Check firewall status:

```bash
sudo ufw status
```

---

# 10. Test FastAPI

Test API inside the server:

```bash
curl http://localhost:8000
```

Test Swagger Docs:

```bash
curl http://localhost:8000/docs
```

Now open in browser:

```
http://SERVER_IP:8000
```

---

# 11. Update the Project
