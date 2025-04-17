# 📊 Application Monitoring Dashboard

A distributed system for logging, processing, and visualizing API request metrics using **FastAPI**, **Kafka**, **MySQL**, and **Grafana**.

---

## 📁 Repository Structure

The `log-analytics/` directory contains:

- `docker-compose.yml`: Defines services – zookeeper, kafka, mysql, api, log_consumer, metrics_generator, and grafana.
- `Dockerfile.api`: Builds the FastAPI container for the API service.
- `Dockerfile.consumer`: Builds the container for log_consumer and metrics_generator.
- `main.py`: FastAPI app with endpoints (`/health`, `/products`, `/users`, `/orders`, `/login`) and Kafka producer.
- `log_consumer.py`: Consumes Kafka messages from `request_logs` topic and stores them in MySQL.
- `metrics_generator.py`: Aggregates logs into metrics (request count, avg response time) every minute.
- `db_init.py`: Initializes MySQL tables (`request_logs`, `metrics`).
- `workload.py`: Simulates API traffic by sending random requests.
- `requirements.txt`: Python dependencies (`fastapi`, `kafka-python`, etc.).
- `mysql.cnf`: Configures MySQL to bind to `0.0.0.0`.
- `start_log_consumer.sh`: Starts `log_consumer.py`.
- `start_metrics.sh`: Starts `metrics_generator.py`.
- `start.sh`: Optional general startup script.
- `.env`: Environment variables for MySQL and Kafka.

---

## 🔁 System Workflow

```plaintext
[workload.py] 
    ↓ 
[FastAPI API] 
    ↓ 
[Kafka] 
    ↓ 
[Log Consumer] → [MySQL] → [Grafana]
             ↓
   [Metrics Generator]


# Prerequisites

Docker

Docker Compose

Python 3.9+

Git


##⚙️ Setup and Execution


1. Clone the Repository

2. Set Up Python Environment

    i.e python3 -m venv venv
	source venv/bin/activate
	pip install -r requirements.txt
 
3. Start All Services

	docker-compose up --build -d

4. Simulate API Load

	source venv/bin/activate
	python3 workload.py

This sends requests to: /health, /products, /users, /orders, /login


##🛠️ Troubleshooting


Check if API is running:

docker-compose logs api(same goes for all like consumer,kafka,metricsgenerator)



##🔌 Kafka Connectivity Issues

If API can't connect to Kafka:
	
	docker-compose exec api bash
	apt update && apt install -y netcat-openbsd
	nc -zv kafka 9092

##📊 Access Grafana Dashboard

Open your browser: http://localhost:3000

Login:

Username: admin

Password: admin


