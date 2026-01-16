# 🦖 Project Eye-Saurus: Full Stack Observability

**Project Eye-Saurus** is a demonstration of a containerized Microservices architecture monitored by a production-grade observability stack. It deploys a "Stranger Things" themed Q&A application alongside a robust monitoring system using **Prometheus**, **Grafana**, and **cAdvisor**.

---

## 🏗️ Architecture

The infrastructure consists of two distinct stacks running entirely on **Docker**:

### 1. The Application Stack
* **Frontend:** A lightweight HTML/JS web interface serving the UI.
* **Backend:** A Python Flask API that processes requests and talks to the database.
* **Database:** A MySQL 5.7 container with persistent volume storage.

### 2. The Monitoring Stack
* **The Spy (cAdvisor):** Watches Docker containers in real-time to gather resource usage metrics (CPU, RAM).
* **The Brain (Prometheus):** A time-series database that scrapes metrics from cAdvisor every 15 seconds.
* **The Face (Grafana):** A visual dashboard that queries Prometheus to display health and performance charts.

---

## 🛠️ Tech Stack

* **Infrastructure:** AWS EC2, Docker, Docker Compose
* **Application:** Node.js (Base), Python (Flask), HTML/CSS
* **Database:** MySQL
* **Observability:** Prometheus, Grafana, cAdvisor

---

## 🚀 Getting Started

### Prerequisites
* Docker & Docker Compose installed.
* An active internet connection (for building images).

### Installation

1.  **Clone the Repository**
    ```bash
    git clone [https://github.com/yashas-nagaraj/eyesaurus.git](https://github.com/yashas-nagaraj/eyesaurus.git)
    cd eyesaurus
    ```

2.  **Configure Networking (Important!)**
    Since the Frontend runs in your browser, it needs to know where the Backend API lives.
    * Open `frontend/index.html`.
    * Find the line: `const API = ...`
    * Replace it with your server's Public IP:
        ```javascript
        const API = "http://<YOUR_PUBLIC_IP>:5000/api";
        ```

3.  **Launch the Stack**
    Run the following command to build the custom images and start all 6 containers:
    ```bash
    docker-compose up -d --build
    ```

4.  **Initialize the Database**
    The database starts empty. Run this one-time command to create the required tables:
    ```bash
    docker exec -it stranger_backend python3 -c "import mysql.connector; import os; conn = mysql.connector.connect(host='db', user='root', password='strangerpassword', database='stranger_db'); cursor = conn.cursor(); cursor.execute('CREATE TABLE IF NOT EXISTS questions (id INT AUTO_INCREMENT PRIMARY KEY, question_text VARCHAR(255))'); cursor.execute('CREATE TABLE IF NOT EXISTS answers (id INT AUTO_INCREMENT PRIMARY KEY, question_id INT, answer_text VARCHAR(255))'); conn.commit(); print('Tables Created'); conn.close()"
    ```

---

## 📊 Monitoring Setup

Once the app is running, set up the dashboards to visualize the data.

1.  **Access Grafana**
    * URL: `http://<YOUR_IP>:3000`
    * **User:** `admin`
    * **Password:** `strangerpassword`

2.  **Connect Data Source**
    * Go to **Connections** > **Data Sources** > **Prometheus**.
    * **Server URL:** `http://prometheus:9090` (Note: Use the container name, not localhost).
    * Click **Save & Test**.

3.  **Import Dashboard**
    * Go to **Dashboards** > **New** > **Import**.
    * Enter Dashboard ID: `14282` (Docker Monitoring).
    * Click **Load**, select your Prometheus source, and hit **Import**.

---

## 🧪 Testing the Project

1.  Open the App at `http://<YOUR_IP>:80`.
2.  Submit questions and answers to generate traffic.
3.  Check Grafana to see real-time CPU spikes and memory usage for the `stranger_backend` and `stranger_db` containers.

---

### 📜 License
This project is open-source and available for educational purposes.
