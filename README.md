# 📈 Real-Time Stock Market Data Pipeline

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?style=for-the-badge&logo=apache-kafka&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=apache-airflow&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=for-the-badge&logo=snowflake&logoColor=white)
![DBT](https://img.shields.io/badge/DBT-FF694B?style=for-the-badge&logo=dbt&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=power-bi&logoColor=black)

**An end-to-end real-time data engineering pipeline built with the Modern Data Stack**

[Features](#-features) • [Architecture](#-architecture) • [Getting Started](#-getting-started) • [Usage](#-usage) • [Contact](#-contact)

</div>

---

## 📌 Overview

This project demonstrates a production-grade real-time data pipeline that captures live stock market data from the Finnhub API, streams it through Apache Kafka, orchestrates transformations with Airflow, and delivers analytics-ready insights in Snowflake. It showcases enterprise-level data engineering practices from ingestion to visualization using the Modern Data Stack.

### 🎯 Key Highlights
- ✅ **Real-time data** from Finnhub API (not simulated)
- ✅ **Event streaming** with Apache Kafka
- ✅ **Cloud data warehouse** with Snowflake
- ✅ **Workflow orchestration** with Apache Airflow
- ✅ **ELT transformations** using DBT (Bronze → Silver → Gold)
- ✅ **Containerized deployment** with Docker
- ✅ **Interactive dashboards** with Power BI

---

## 🏗️ Architecture

![Architecture Diagram](Architecture.png)

The pipeline implements a **medallion architecture** with three data quality layers:

**Data Flow:**
1. **Ingestion Layer**: Python producer fetches live stock quotes from Finnhub API
2. **Streaming Layer**: Apache Kafka streams data in real-time across 3 partitions  
3. **Storage Layer**: Kafka consumer stores raw data in MinIO (S3-compatible storage)
4. **Orchestration Layer**: Airflow DAG loads data from MinIO to Snowflake every minute
5. **Transformation Layer**: DBT models transform data across Bronze → Silver → Gold layers
6. **Presentation Layer**: Power BI connects to Gold layer for interactive analytics

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Data Source** | Finnhub API | Live stock market data |
| **Streaming** | Apache Kafka | Real-time message streaming |
| **Object Storage** | MinIO | S3-compatible raw data storage |
| **Orchestration** | Apache Airflow | Workflow automation & scheduling |
| **Data Warehouse** | Snowflake | Cloud-based data warehouse |
| **Transformation** | DBT (Data Build Tool) | SQL-based ELT transformations |
| **Visualization** | Power BI | Business intelligence dashboards |
| **Programming** | Python 3.8+ | Producer/Consumer application logic |
| **Containerization** | Docker & Docker Compose | Service deployment |

---

## ✨ Features

### 🔄 Real-Time Streaming
- Kafka producer continuously fetches stock data from Finnhub API
- Multi-partition Kafka topic (3 partitions) for parallel processing
- Kafka consumer streams data to MinIO storage in real-time
- Kafdrop UI for monitoring Kafka topics and messages

### 🤖 Automated Orchestration
- Airflow DAG schedules data ingestion every 1 minute
- Automated error handling with retry logic
- Email alerts on task failures
- Visual workflow monitoring through Airflow web UI

### 🎨 Multi-Layer Data Transformations
- **Bronze Layer**: Raw data ingestion (staging tables)
- **Silver Layer**: Cleaned, validated, and deduplicated data
- **Gold Layer**: Business-ready analytics models
  - `gold_candlestick`: OHLC data for candlestick charts
  - `gold_kpi`: Key performance indicators and metrics
  - `gold_treechart`: Hierarchical stock comparison data

### 📊 Analytics & Visualization
- Interactive Power BI dashboards with Direct Query
- Real-time stock price monitoring
- Volume analysis and trend visualization
- Candlestick patterns for technical analysis

### ⚡ Scalable Architecture
- Fully containerized with Docker Compose
- Cloud-native data warehouse (Snowflake)
- Horizontal scaling capability with Kafka partitions
- Stateless microservices design

---

## 📂 Project Structure

```
real-time-stocks-pipeline/
│
├── infra/
│   └── docker-compose.yml              # Service orchestration config
│
├── producer/
│   └── producer.py                     # Kafka producer (Finnhub integration)
│
├── consumer/
│   └── consumer.py                     # Kafka consumer (MinIO sink)
│
├── dbt_stocks/
│   ├── models/
│   │   ├── bronze/
│   │   │   ├── bronze_stg_stock_quotes.sql
│   │   │   └── sources.yml
│   │   ├── silver/
│   │   │   └── silver_clean_stock_quotes.sql
│   │   └── gold/
│   │       ├── gold_candlestick.sql
│   │       ├── gold_kpi.sql
│   │       └── gold_treechart.sql
│   ├── dbt_project.yml
│   └── profiles.yml
│
├── dag/
│   └── minio_to_snowflake.py           # Airflow DAG definition
│
├── requirements.txt                     # Python dependencies
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

**Software Requirements:**
- Docker Desktop (latest version)
- Python 3.8 or higher
- Git

**Account Requirements:**
- [Snowflake](https://signup.snowflake.com/) account (free trial available)
- [Finnhub](https://finnhub.io/register) API key (free tier)
- Power BI Desktop (for visualization)

### Installation Steps

#### 1️⃣ Clone the Repository
```bash
git clone https://github.com/yourusername/real-time-stocks-pipeline.git
cd real-time-stocks-pipeline
```

#### 2️⃣ Set Up Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

#### 3️⃣ Install Python Dependencies
```bash
pip install -r requirements.txt
```

#### 4️⃣ Start Docker Services
```bash
cd infra
docker-compose up -d
```

This will start:
- Apache Kafka
- Zookeeper
- Kafdrop (Kafka UI)
- MinIO (Object Storage)
- PostgreSQL (Airflow metadata DB)
- Airflow Webserver
- Airflow Scheduler

#### 5️⃣ Initialize Airflow Database
```bash
docker-compose run --rm airflow-webserver airflow db init
docker-compose up -d airflow-scheduler airflow-webserver
```

#### 6️⃣ Create Airflow Admin User
```bash
docker-compose exec airflow-webserver airflow users create \
  --username admin \
  --firstname Admin \
  --lastname User \
  --role Admin \
  --email admin@example.com \
  --password admin123
```

---

## ⚙️ Configuration

### 1. Finnhub API Setup

1. Sign up at [Finnhub.io](https://finnhub.io/register)
2. Get your free API key
3. Update `producer/producer.py`:

```python
FINNHUB_API_KEY = "your_finnhub_api_key_here"
STOCK_SYMBOLS = ["AAPL", "GOOGL", "MSFT", "AMZN", "TSLA"]  # Customize symbols
```

### 2. Kafka Topic Creation

1. Access Kafdrop UI: `http://localhost:9000`
2. Create a new topic:
   - **Topic Name**: `stock_quotes`
   - **Partitions**: `3`
   - **Replication Factor**: `1`

### 3. MinIO Configuration

1. Access MinIO Console: `http://localhost:9001`
2. Login credentials (from `docker-compose.yml`):
   - **Username**: `minioadmin`
   - **Password**: `minioadmin`
3. Create bucket: `stocks-data`

### 4. Snowflake Setup

Execute the following SQL commands in Snowflake:

```sql
-- Create database and schema
CREATE DATABASE STOCKS_DB;
CREATE SCHEMA STOCKS_DB.RAW_DATA;
USE SCHEMA STOCKS_DB.RAW_DATA;

-- Create warehouse
CREATE WAREHOUSE STOCKS_WH 
  WITH WAREHOUSE_SIZE = 'XSMALL' 
  AUTO_SUSPEND = 60 
  AUTO_RESUME = TRUE;

-- Create staging table
CREATE TABLE STOCK_QUOTES_STAGING (
  symbol STRING,
  current_price FLOAT,
  change FLOAT,
  percent_change FLOAT,
  high FLOAT,
  low FLOAT,
  open FLOAT,
  previous_close FLOAT,
  timestamp TIMESTAMP_NTZ
);
```

### 5. DBT Configuration

1. Navigate to DBT directory:
```bash
cd dbt_stocks
```

2. Initialize DBT (if not already done):
```bash
dbt init
```

3. Update `~/.dbt/profiles.yml` with your Snowflake credentials:

```yaml
dbt_stocks:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: your_account.region  # e.g., abc12345.us-east-1
      user: your_username
      password: your_password
      role: ACCOUNTADMIN
      database: STOCKS_DB
      warehouse: STOCKS_WH
      schema: RAW_DATA
      threads: 4
      client_session_keep_alive: False
```

4. Test connection:
```bash
dbt debug
```

---

## 💻 Running the Pipeline

### Step 1: Start the Producer 🚀

Open a terminal and run:

```bash
source venv/bin/activate
python producer/producer.py
```

**Expected Output:**
```
Connected to Finnhub API
Fetching stock data for: AAPL, GOOGL, MSFT, AMZN, TSLA
Sent message to Kafka: {"symbol": "AAPL", "price": 178.52, ...}
```

✅ **Verify**: Check Kafdrop at `http://localhost:9000` → Topics → `stock_quotes` → Messages

### Step 2: Start the Consumer 📥

Open a new terminal and run:

```bash
source venv/bin/activate
python consumer/consumer.py
```

**Expected Output:**
```
Connected to Kafka consumer
Consuming from topic: stock_quotes
Saved message to MinIO: stocks-data/raw/2025-01-03/stock_data_001.json
```

✅ **Verify**: Check MinIO at `http://localhost:9001` → Buckets → `stocks-data` → Browse files

### Step 3: Enable Airflow DAG 🔄

1. Access Airflow UI: `http://localhost:8080`
2. Login with credentials:
   - **Username**: `admin`
   - **Password**: `admin123`
3. Find DAG: `minio_to_snowflake`
4. Toggle the switch to enable it
5. Click "Trigger DAG" to run manually (or wait for scheduled run)

✅ **Verify**: Check DAG runs and task logs in Airflow UI

### Step 4: Run DBT Transformations 🎨

```bash
cd dbt_stocks
dbt run
```

**Expected Output:**
```
Running with dbt=1.7.0
Found 5 models, 0 tests, 0 snapshots...

Completed successfully
```

✅ **Verify**: Check Snowflake for tables:
- `STOCKS_DB.RAW_DATA.BRONZE_STG_STOCK_QUOTES`
- `STOCKS_DB.RAW_DATA.SILVER_CLEAN_STOCK_QUOTES`
- `STOCKS_DB.RAW_DATA.GOLD_CANDLESTICK`
- `STOCKS_DB.RAW_DATA.GOLD_KPI`
- `STOCKS_DB.RAW_DATA.GOLD_TREECHART`

### Step 5: Connect Power BI 📊

1. Open Power BI Desktop
2. Click **Get Data** → **More** → Search "Snowflake"
3. Enter connection details:
   - **Server**: `your_account.region.snowflakecomputing.com`
   - **Warehouse**: `STOCKS_WH`
4. Select **DirectQuery** mode
5. Navigate to: `STOCKS_DB` → `RAW_DATA` → Select Gold tables
6. Click **Load**
7. Create visualizations:
   - Candlestick chart using `gold_candlestick`
   - KPI cards using `gold_kpi`
   - Tree map using `gold_treechart`

---

## 📊 Data Models

### Bronze Layer (Raw Zone)
**Table**: `bronze_stg_stock_quotes`

```sql
-- Raw data as ingested from Kafka/MinIO
-- No transformations applied
-- Includes: symbol, current_price, change, percent_change, high, low, open, previous_close, timestamp
```

### Silver Layer (Cleaned Zone)
**Table**: `silver_clean_stock_quotes`

```sql
-- Data quality checks applied
-- Transformations:
--   • Remove null/invalid records
--   • Standardize data types
--   • Remove duplicates based on symbol + timestamp
--   • Add derived columns (price_range, volatility_indicator)
```

### Gold Layer (Analytics Zone)

**1. gold_candlestick**
```sql
-- OHLC data for candlestick visualizations
-- Aggregated by: symbol, date
-- Columns: symbol, date, open, high, low, close, volume
```

**2. gold_kpi**
```sql
-- Key performance indicators
-- Columns: symbol, current_price, daily_change, daily_change_pct, 
--          volume, market_cap, pe_ratio, week_high_52, week_low_52
```

**3. gold_treechart**
```sql
-- Hierarchical stock comparison data
-- Grouped by: sector, industry, symbol
-- Metrics: market_cap, daily_change_pct, volume
```

---

## 🐳 Docker Services

### Service Overview

| Service | Port | Purpose | Access |
|---------|------|---------|--------|
| **Zookeeper** | 2181 | Kafka cluster coordination | Internal |
| **Kafka** | 9092 | Message broker | Internal |
| **Kafdrop** | 9000 | Kafka UI | http://localhost:9000 |
| **MinIO** | 9000, 9001 | S3-compatible storage | http://localhost:9001 |
| **PostgreSQL** | 5432 | Airflow metadata DB | Internal |
| **Airflow Webserver** | 8080 | Workflow UI | http://localhost:8080 |
| **Airflow Scheduler** | - | DAG execution engine | Background |

### Useful Docker Commands

```bash
# View all running containers
docker-compose ps

# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs for specific service
docker-compose logs -f kafka
docker-compose logs -f airflow-scheduler

# Restart a specific service
docker-compose restart kafka

# Remove all containers and volumes
docker-compose down -v

# Execute command inside container
docker-compose exec kafka bash
```

---

## 🔧 Troubleshooting

### Issue 1: Airflow Services Keep Restarting

**Solution:**
```bash
docker-compose run --rm airflow-webserver airflow db init
docker-compose restart airflow-scheduler airflow-webserver
```

### Issue 2: Kafka Producer Connection Error

**Symptoms:** `NoBrokersAvailable` or `Connection refused`

**Solution:**
1. Check if Kafka is running: `docker-compose ps kafka`
2. Verify Kafka logs: `docker-compose logs kafka`
3. Ensure Zookeeper is healthy: `docker-compose logs zookeeper`
4. Restart Kafka: `docker-compose restart kafka`

### Issue 3: DBT Can't Connect to Snowflake

**Symptoms:** `Database Error` or `Connection timeout`

**Solution:**
1. Verify `profiles.yml` has correct credentials
2. Test connection: `dbt debug`
3. Check Snowflake warehouse is running:
   ```sql
   SHOW WAREHOUSES;
   -- If suspended, resume it:
   ALTER WAREHOUSE STOCKS_WH RESUME;
   ```
4. Verify network connectivity to Snowflake

### Issue 4: MinIO Access Denied

**Solution:**
1. Check credentials in `docker-compose.yml`
2. Recreate bucket with correct permissions
3. Restart MinIO: `docker-compose restart minio`

### Issue 5: Producer Not Fetching Data

**Symptoms:** No messages in Kafka topic

**Solution:**
1. Verify Finnhub API key is valid
2. Check API rate limits (free tier: 60 calls/minute)
3. Test API manually:
   ```bash
   curl "https://finnhub.io/api/v1/quote?symbol=AAPL&token=YOUR_API_KEY"
   ```
4. Check producer logs for errors

---

## 📈 Power BI Dashboard

### Dashboard Components

**1. Candlestick Chart**
- Source: `gold_candlestick` table
- X-axis: Date
- Y-axis: OHLC (Open, High, Low, Close)
- Shows price patterns and trends

**2. KPI Cards**
- Source: `gold_kpi` table
- Metrics: Current Price, Daily Change %, Volume, Market Cap
- Color-coded (green for positive, red for negative)

**3. Tree Map**
- Source: `gold_treechart` table
- Hierarchy: Sector → Industry → Symbol
- Size: Market Cap
- Color: Daily Change %

### Refresh Settings

For real-time updates, configure DirectQuery mode:
1. File → Options → Data Load
2. Select "DirectQuery"
3. Set auto-refresh interval (minimum 15 seconds for Pro license)

---

## 🎯 Future Enhancements

- [ ] Add sentiment analysis from Twitter/Reddit
- [ ] Implement real-time price alerts via email/SMS
- [ ] Add machine learning models for price prediction
- [ ] Integrate news API for fundamental analysis
- [ ] Deploy to cloud (AWS ECS/EKS or GCP Cloud Run)
- [ ] Add data quality monitoring with Great Expectations
- [ ] Implement CI/CD pipeline with GitHub Actions
- [ ] Add support for cryptocurrency and forex data
- [ ] Create mobile app for real-time notifications
- [ ] Add backtesting framework for trading strategies

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 Jaya Chandra Kadiveti

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---

## 👨‍💻 Author

**Jaya Chandra Kadiveti**

- 📧 Email: [datawithjay1@gmail.com](mailto:datawithjay1@gmail.com)
- 💼 LinkedIn: [Jaya Chandra Kadiveti](https://linkedin.com/in/jayachandrakadiveti)
- 🐙 GitHub: [@jayachandrak](https://github.com/jayachandrak)
- 📝 Blog: [Data Engineering Insights](https://yourblog.com)

---

## 🙏 Acknowledgments

- [Finnhub.io](https://finnhub.io/) for providing free stock market API
- [Snowflake](https://www.snowflake.com/) for cloud data platform
- [DBT Labs](https://www.getdbt.com/) for transformation framework
- [Apache Software Foundation](https://www.apache.org/) for Kafka and Airflow
- [MinIO](https://min.io/) for S3-compatible object storage

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### How to Contribute

1. Fork the repository
2. Create your feature branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. Commit your changes
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. Push to the branch
   ```bash
   git push origin feature/AmazingFeature
   ```
5. Open a Pull Request

### Contribution Guidelines

- Follow PEP 8 style guide for Python code
- Add unit tests for new features
- Update documentation as needed
- Ensure all tests pass before submitting PR

---

## 📞 Support

If you have any questions or need help with the project:

- 📧 Email: datawithjay1@gmail.com
- 💬 Open an [Issue](https://github.com/yourusername/real-time-stocks-pipeline/issues)
- 💼 Connect on [LinkedIn](https://linkedin.com/in/jayachandrakadiveti)

---

## ⭐ Show Your Support

If you found this project helpful or interesting, please consider:

- ⭐ Starring the repository
- 🐛 Reporting bugs or issues
- 💡 Suggesting new features
- 🔀 Forking and contributing
- 📢 Sharing with others

---

<div align="center">

### 🚀 Built with passion for Data Engineering 🚀

**Made with ❤️ by Jaya Chandra Kadiveti**

⭐ **Star this repo if you found it useful!** ⭐

</div>
