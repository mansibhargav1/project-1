# 🚀 Flask App Deployment on GCP VM (Dockerized)

This project demonstrates how to **containerize a Flask application** and deploy it manually on a **GCP Ubuntu VM using Docker**.

---

## 📌 Architecture

```
User → GCP VM (Ubuntu) → Docker Container → Flask App → Browser (Port 80)
```

---

## 🧰 Tech Stack

* Python 3.14
* Flask 3.1
* Docker (Multi-stage build)
* GCP Compute Engine (Ubuntu VM)

---

## ⚙️ Step 1: Create GCP VM

1. Go to **Compute Engine → VM Instances**
2. Click **Create Instance**

### Configuration:

* Name: `flask-app-vm`
* Region: `us-central1`
* Machine: `e2-micro`
* OS: **Ubuntu 22.04**
* Enable:

  * ✅ Allow HTTP Traffic
  * ✅ Allow HTTPS Traffic

---

## 📸 VM Instance Created

![VM Instance](./screenshots/vm-instance.png)

---

## 🔐 Step 2: Connect via SSH

Click **SSH** button from GCP console.

---

## 🐳 Step 3: Install Docker

```bash
sudo apt update
sudo apt install -y docker.io

sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker

sudo usermod -aG docker $USER
newgrp docker
```

---

## 📥 Step 4: Clone Repository

```bash
git clone https://github.com/mansibhargav1/dockerise-app.git
cd dockerise-app
```

---

## 🏗 Step 5: Build Docker Image

### Option 1: Build locally (basic)

```bash
docker build -f Dockerfile-multi -t flask-app .
```

---

### Option 2: Build with your own Docker Registry (Recommended 🚀)

Replace `<REGISTRY-IP>` and `<PORT>` with your Docker registry details.

```bash
docker build -f Dockerfile-multi -t <REGISTRY-IP>:<PORT>/flask-app:latest .
```

### Example:

```bash
docker build -f Dockerfile-multi -t 136.112.56.210:5000/flask-app:latest .
```

---

👉 This will directly tag your image for your private registry, so you can push it easily:

```bash
docker push <REGISTRY-IP>:<PORT>/flask-app:latest
```
<img width="1433" height="538" alt="image" src="https://github.com/user-attachments/assets/dcde0e39-dff1-4844-b925-0a79bcb67d58" />


---

## ▶️ Step 6: Run Container

```bash
docker run -d -p 80:80 flask-app
```

Check running container:

```bash
docker ps
```

---

## 🌐 Step 7: Access Application

Open browser:

```
http://<EXTERNAL_IP>
```

---

## 📸 Application Output

![App UI](./screenshots/app-ui.png)
<img width="1427" height="853" alt="image" src="https://github.com/user-attachments/assets/b48e9e66-d5b8-484c-be3a-1e2e97183767" />

---

## ⚠️ Troubleshooting

### App not opening?

✔ Check container:

```bash
docker ps
```

✔ Check logs:

```bash
docker logs <container_id>
```

✔ Check firewall:

* Go to **VPC → Firewall**
* Allow TCP: `80`

---

## 🚀 Optional: Push to GCP Artifact Registry

### Enable API:

```bash
gcloud services enable artifactregistry.googleapis.com
```

### Create repo:

```bash
gcloud artifacts repositories create flask-repo \
  --repository-format=docker \
  --location=us-central1
```

### Authenticate:

```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

### Tag & Push:

```bash
docker tag flask-app us-central1-docker.pkg.dev/<PROJECT_ID>/flask-repo/flask-app:v1
docker push us-central1-docker.pkg.dev/<PROJECT_ID>/flask-repo/flask-app:v1
```

---

## 💡 Key Learnings

* Containerized application using Docker (multi-stage build)
* Deployed app on GCP VM manually
* Exposed application using public IP
* Configured Docker runtime on Ubuntu
* Understood basic cloud networking & firewall rules

---

## 📂 Project Structure

```
.
├── app.py
├── run.py
├── Dockerfile
├── Dockerfile-multi
├── requirements.txt
└── templates/
```

---

## 👨‍💻 Author

**Mansi 🚀**

---
