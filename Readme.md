# 🚗 Moroccan Automotive Market — Real-Time Data Engineering Pipeline

A comprehensive **End-to-End Data Pipeline** designed to scrape, process, and visualize real-time data from the Moroccan automotive market. The system provides actionable insights into vehicle pricing trends, brand popularity, and market fluctuations across Moroccan cities.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [System Architecture](#system-architecture)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Key Features](#key-features)
- [Sample Insights](#sample-insights)
- [Roadmap](#roadmap)

---

## Overview

This project automates the collection of automotive listing data from Moroccan classifieds, streams it through a distributed messaging system, processes it with Apache Spark, and stores it across multiple storage layers — enabling both real-time dashboards and batch analytics.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Scraping** | Python, Selenium, BeautifulSoup |
| **Orchestration** | Apache Airflow |
| **Streaming** | Apache Kafka |
| **Processing** | Apache Spark (Streaming & Batch) |
| **Data Lake** | Hadoop HDFS |
| **NoSQL Storage** | Apache Cassandra, MongoDB |
| **SQL Storage** | MySQL |
| **Visualization** | Tableau, Power BI |
| **Infrastructure** | Docker, Linux / Bash |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  1. EXTRACTION                                               │
│     Selenium + BeautifulSoup → Moroccan auto classifieds     │
│     Scheduled by Apache Airflow DAGs                         │
└────────────────────────┬────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────┐
│  2. INGESTION                                                │
│     Kafka Producer → Kafka Topics → Kafka Consumer           │
│     Decoupled, fault-tolerant streaming layer                │
└────────────────────────┬────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────┐
│  3. PROCESSING                                               │
│     Spark Streaming → real-time cleaning & transformation    │
│     Spark Batch    → historical aggregations & analytics     │
└──────────────┬──────────────────────────┬───────────────────┘
               │                          │
┌──────────────▼───────┐    ┌─────────────▼──────────────────┐
│  4. RAW STORAGE       │    │  5. PROCESSED STORAGE           │
│     Hadoop HDFS       │    │     Cassandra (high-velocity)   │
│     (Data Lake)       │    │     MongoDB   (documents)       │
└───────────────────────┘    │     MySQL     (analytics/SQL)   │
                             └─────────────┬──────────────────┘
                                           │
┌──────────────────────────────────────────▼──────────────────┐
│  6. VISUALIZATION                                            │
│     Tableau / Power BI — dashboards, trend charts, reports   │
└─────────────────────────────────────────────────────────────┘
```

### Flow Summary

1. **Data Extraction** — Automated scrapers target Moroccan automotive classifieds and extract listing data (price, brand, model, year, mileage, city).
2. **Ingestion** — Scraped data is published to Kafka topics, providing a decoupled and fault-tolerant streaming buffer.
3. **Processing** — Spark Streaming consumes messages for real-time cleaning and transformation; Spark Batch handles scheduled aggregations.
4. **Storage** — Raw data is archived in HDFS. Processed data is written to Cassandra for fast lookups and MySQL for relational queries.
5. **Visualization** — Business dashboards and reports are served via Tableau or Power BI.

---

## 📂 Project Structure

```
├── airflow/
│   ├── dags/                  # Airflow DAG definitions
│   └── plugins/               # Custom operators & hooks
├── scrapers/
│   ├── selenium_scraper.py    # Dynamic page scraper
│   └── bs4_parser.py          # HTML parsing utilities
├── kafka/
│   ├── producer.py            # Kafka producer config & logic
│   └── consumer.py            # Kafka consumer config & logic
├── spark/
│   ├── streaming_job.py       # Spark Streaming processing
│   └── batch_job.py           # Spark Batch aggregation jobs
├── storage/
│   ├── cassandra_schema.cql   # Cassandra table definitions
│   ├── mongo_schema.json      # MongoDB collection schema
│   └── mysql_schema.sql       # MySQL table definitions
├── docker-compose.yml         # Full containerized environment
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Docker & Docker Compose
- Python 3.9+
- Java 11+ (for Spark & Kafka)

### Run the Stack

```bash
# Clone the repository
git clone https://github.com/your-username/moroccan-auto-pipeline.git
cd moroccan-auto-pipeline

# Start all services
docker-compose up -d

# Access Airflow UI
open http://localhost:8080

# Trigger the scraping DAG manually
airflow dags trigger automotive_scraper_dag
```

---

## ✨ Key Features

- **Real-Time Processing** — Captures market changes as they happen via Kafka + Spark Streaming.
- **Automated Pipelines** — Airflow DAGs manage scraping schedules and Spark job execution.
- **Scalable Architecture** — Designed to handle high volumes of listings across multiple Moroccan cities.
- **Multi-Layer Storage** — Raw archiving (HDFS), fast lookups (Cassandra), and relational analytics (MySQL).
- **ML-Ready Data** — Processed data is structured for easy integration with machine learning price prediction models.

---

## 📊 Sample Insights

> *"In 2025, the resale value of hybrid vehicles in the Casablanca-Settat region saw a 12% increase compared to diesel counterparts."*

Other example queries the pipeline supports:

- Average resale price of a **Dacia Sandero** by year and city
- Most listed brands in **Marrakech vs Casablanca**
- Price depreciation curve for **diesel vs hybrid** vehicles
- Seasonal listing volume trends across Moroccan regions

---

## 🗺️ Roadmap

- [ ] Add ML price prediction model (scikit-learn / PySpark MLlib)
- [ ] Expand scraping coverage to additional Moroccan platforms
- [ ] Add real-time alerting for significant price drops
- [ ] Integrate a REST API layer for external data access
- [ ] Add data quality monitoring with Great Expectations

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
