# 🚀 FastAPI DevOps Project (Docker + Jenkins + Monitoring)

## 📌 Project Overview

This project demonstrates a complete **DevOps workflow** by deploying a FastAPI application using:

* 🐳 Docker & Docker Compose
* ⚙️ Jenkins CI/CD Pipeline
* 📊 Prometheus (Monitoring)
* 📈 Grafana (Visualization)

The application stores user data in a JSON file and ensures **data persistence using Docker volumes**.

---

## 🛠️ Tech Stack

* **Backend**: FastAPI (Python)
* **Containerization**: Docker, Docker Compose
* **CI/CD**: Jenkins
* **Monitoring**: Prometheus
* **Visualization**: Grafana
* **Cloud**: AWS EC2

---

## 📂 Project Structure

```
fastApi/
│── app/
│   ├── main.py
│   ├── services.py
│   ├── schema.py
│   └── data/
│
│── docker-compose.yml
│── Dockerfile
│── requirements.txt
│── README.md
```

---

## 🚀 Application Features

| Method | Endpoint   | Description             |
| ------ | ---------- | ----------------------- |
| GET    | `/`        | Returns welcome message |
| GET    | `/users`   | Fetch all users         |
| POST   | `/users`   | Add new user            |
| GET    | `/metrics` | Prometheus metrics      |

---

## 🐳 Docker Setup

### 🔧 Build & Run

```
docker-compose up --build -d
```

### 🌐 Access API

```
http://<EC2-IP>:8000/docs
```

---

## 💾 Data Persistence

* Data stored in: `app/data/users.json`
* Docker volume ensures data is **not lost after container restart**

---

## 🔁 CI/CD with Jenkins

### Pipeline Stages:

1. Clone GitHub Repository
2. Build Docker Image
3. Stop old container
4. Deploy new container

### Jenkinsfile

```
pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/rohitatole29-collab/fastApi.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t fastapi-app .'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                docker rm -f fastapi || true
                docker run -d -p 8000:8000 \
                -v $(pwd)/app/data:/code/app/data \
                --name fastapi fastapi-app
                '''
            }
        }
    }
}
```

---

## 📊 Monitoring with Prometheus

### Run Prometheus

```
docker run -d -p 9090:9090 \
-v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
--name prometheus prom/prometheus
```

### Access

```
http://<EC2-IP>:9090
```

---

## 📈 Grafana Dashboard

### Run Grafana

```
docker run -d -p 3000:3000 --name grafana grafana/grafana
```

### Access

```
http://<EC2-IP>:3000
```

### Default Login

* Username: `admin`
* Password: `admin`

---

## ⚙️ Grafana Setup

* Add Data Source → Prometheus
* URL: `http://<EC2-IP>:9090`
* Create dashboard with query:

```
request_count
```

---

## 📊 Metrics Endpoint

FastAPI exposes metrics at:

```
/metrics
```

---

## ☁️ AWS Deployment

* Deployed on EC2 instance
* Security groups configured for:

  * 8000 (FastAPI)
  * 8080 (Jenkins)
  * 9090 (Prometheus)
  * 3000 (Grafana)

---

## ✅ Key Highlights

✔ Dockerized FastAPI application
✔ CI/CD pipeline using Jenkins
✔ Data persistence using volumes
✔ Real-time monitoring with Prometheus
✔ Visualization with Grafana

---

## 💼 Use Case

This project demonstrates how to build a **production-ready DevOps pipeline** with monitoring and automation.

---

## 👨‍💻 Author

**Rohit Atole**

---

## ⭐ Support

If you like this project, give it a ⭐ on GitHub!
