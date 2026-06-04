# IPL Live Cricket Data Analytics Pipeline 🏏📊

This project is an automated data pipeline that collects, cleans, and analyzes live IPL cricket stats. It automatically downloads raw match files, processes them to calculate key player performance metrics (like batting averages and strike rates), and stores the clean data in an optimized format in an AWS data lake so it can be easily visualized.

---

## 🏗️ How the Data Flows

The project is split into two clean phases—keeping the raw historical data completely separate from final analytics data:

1. **Ingestion (Raw Layer):** Every alternate day at 4:00 PM IST, Apache Airflow triggers our Python script. The script downloads a live ZIP archive containing all IPL match history from Cricsheet and uploads the untouched file straight into an Amazon S3 folder named `/raw/`.
2. **Transformation (Analytics Layer):** The pipeline reads that raw ZIP file back into memory, extracts only the matches for target season (2026), and uses Pandas to clean up the columns. It aggregates the data to calculate advanced player metrics and saves the final data back to S3 in the `/analytics_ready/` folder as highly optimized, Snappy-compressed **Apache Parquet** files.
3. **Querying:** Instead of managing an expensive database server, **Amazon Athena** projects a schema on top of our clean Parquet files. 
---

## 🛠️ The Tech Stack

* **Orchestration:** Apache Airflow (running locally in Docker Desktop containers)
* **Code & Data Processing:** Python (Pandas, Numpy, Boto3)
* **Cloud Storage:** Amazon S3 (Data Lake setup)
* **File Format Optimization:** Parquet format with Snappy Compression
* **Query Engine:** Amazon Athena (Serverless SQL)

---

## 📁 Repository Structure

```text
└── DAG File  # The main Apache Airflow DAG script containing the ETL logic
