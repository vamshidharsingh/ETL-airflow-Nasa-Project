# 🚀 NASA APOD ETL Pipeline with Apache Airflow & Docker

## 🌌 Overview

This project showcases an end-to-end **ETL pipeline** that fetches data from **NASA’s Astronomy Picture of the Day (APOD)** API, transforms it, and loads it into a **PostgreSQL** database. The entire pipeline is orchestrated using **Apache Airflow** and containerized with **Docker** for portability and reproducibility.

If you're a beginner, don’t worry—this README walks through every step in simple terms!

---

## 📚 Tech Stack

* **Apache Airflow** – Workflow orchestration
* **Docker + Docker Compose** – Containerization
* **PostgreSQL** – Data storage
* **NASA APOD API** – Data source
* **Python** – Data transformation logic

---

## 🎯 Project Goals

* Automate daily data extraction from NASA's APOD API
* Process and clean the data
* Store structured results in PostgreSQL
* Run the entire pipeline in isolated Docker containers

---

## 📈 ETL Pipeline Architecture

```text
          +------------+           +-------------+           +--------------+
          |  NASA API  |  --->     |  Airflow DAG |  --->     | PostgreSQL DB|
          +------------+           +-------------+           +--------------+
                (Extract)               (Transform)                (Load)
```

---

## 🧠 Why NASA APOD?

NASA’s Astronomy Picture of the Day is a treasure trove for space enthusiasts. Each entry includes:

* A stunning image
* Title & description
* Media metadata

The goal? Automate the data collection and build a personal archive for future exploration or app development.

---

## 🛠️ Step-by-Step Pipeline

### 1. 🛸 Setup Apache Airflow (Dockerized)

* Apache Airflow was run inside a Docker container using Docker Compose.
* Defined a custom **DAG** (`nasa_apod_dag`) to orchestrate extraction, transformation, and loading tasks.

### 2. 🌍 Extract Data from NASA API

Used `SimpleHttpOperator` to:

* Send a GET request to the NASA APOD endpoint
* Parse the JSON response (title, explanation, URL, date, media type)

```python
extract_apod = SimpleHttpOperator(
    task_id='extract_apod',
    http_conn_id='nasa_api',
    endpoint='planetary/apod',
    method='GET',
    data={"api_key": "YOUR_API_KEY"},
    response_filter=lambda response: response.json(),
)
```

---

### 3. 🔧 Transform the Data

Extracted relevant fields into a clean dictionary format:

```python
@task
def transform_apod_data(response):
    return {
        'title': response.get('title', ''),
        'explanation': response.get('explanation', ''),
        'url': response.get('url', ''),
        'date': response.get('date', ''),
        'media_type': response.get('media_type', '')
    }
```

---

### 4. 🗄️ Load into PostgreSQL

Used Airflow’s `PostgresHook` to insert data with duplicate handling using `ON CONFLICT DO NOTHING`.

---

### 5. 🐳 Containerized with Docker

* All services (Airflow, PostgreSQL) are spun up using `docker-compose.yml`
* Each container runs in an isolated environment but on a shared network
* Local setup simulates a mini production-like data pipeline

---

## 🧪 Lessons Learned

* Docker Networking: Ensured inter-container communication for Airflow and PostgreSQL
* Airflow DAGs: Learned how to schedule and debug tasks efficiently
* SQL Conflict Resolution: Applied `ON CONFLICT` to avoid duplicate records
* Error Handling: Debugged issues like connection failures, typos in IDs, and invalid column references

---



---

## 🔗 Live Repository

Check out the full project code here:
👉 [GitHub Repo](https://github.com/srujank18/etl-nasa-airflow.git)

---

## 👨‍💻 Future Improvements

* Deploy on AWS/GCP with Cloud Composer or MWAA
* Build a dashboard to visualize daily APOD entries
* Add historical data scraping from APOD archives

---

## ✨ Final Thoughts

This project was not just about building a data pipeline — it was about learning how to solve problems and  i learned  how to  connect tools, and more over  debug . From broken containers to failing SQL inserts, I learned persistence is the real superpower in tech.




