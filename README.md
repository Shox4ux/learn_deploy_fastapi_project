
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

## 4.1 Create keyrings folder

```bash
sudo mkdir -p /etc/apt/keyrings
```

## 4.2 Add docker gpg key (correct way)

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## 4.3 Add docker repository for ubuntu 22.04 jammy

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu jammy stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```


## 4.4 Update package list

```bash
sudo apt update
```


## 4.5 install docker

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


## 4.6 Check Docker installation:

```bash
docker --version
docker compose version
```



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

Clone your repository (replace USERNAME and REPO_NAME):

```bash
git clone https://github.com/USERNAME/REPO_NAME.git
```

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

# 9. Test FastAPI

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

