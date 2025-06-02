# NycTaxi-Azure-Data-Engineer-Project

HomePage API URL: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

base Url -> https://d37ci6vzurychx.cloudfront.net

relative url -> trip-data/green_tripdata_2024-01.parquet

full api url --> https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2024-01.parquet

But only we need, base url and relative url.


------------------------------------------------------------------------------------

Modern Data Lakehouse Architecture on Azure using multiple services:

      Azure Data Factory (ADF) – for orchestration and data ingestion.

      Azure Data Lake Storage Gen2 (ADLS Gen2) – for scalable storage.

      Azure Databricks (ADB) – for data transformation using PySpark/SQL.

      Azure Entra ID (App Registrations) – for secure API access.

      NYC Taxi API – as the external data source.

      Delta Lake format – for ACID-compliant storage and analytics.



End-to-End Pipeline Workflow
------------------------------
🟡 1. Data Ingestion (Bronze Layer)
Source: NYC Taxi Public API 

Ingestion Tool: Azure Data Factory


Method:

    Used HTTP Linked Service to connect to the API.

    Used Copy Activity or Web Activity + Copy to pull JSON/CSV data.

    Authentication: Managed through Azure Entra ID and App Registration.

    App registration provides Client ID, Secret, and Tenant ID.

    Destination: Raw data is saved in Bronze Layer of ADLS Gen2.

    Format: Stored as Parquet files for efficient storage and schema evolution.

    

⚙️ 2. Data Transformation (Silver Layer)
Tool: Azure Databricks (ADB)

Notebook or Job:

    Read Parquet files from Bronze layer.

    Perform data cleansing, filtering, and structuring (e.g., renaming columns, dropping nulls).

    Add business rules like:

    Filtering rides by year/month.

    Normalizing fare or trip distances.

    Write Back: Transformed data is saved into the Silver Layer of ADLS.

    Format: Still in Parquet, but now cleaned and structured.
    

🟢 3. Query-Ready & Sharable (Gold Layer / Delta Table)
Objective: Enable querying by Data Analysts & BI Teams.

Step:

    Read Silver Layer data in Databricks.

    Further enrichment (e.g., joining datasets, aggregation).

    Save final dataset as Delta Tables in a Databricks Database.

    Delta Lake Benefits:

    Supports ACID transactions, Time Travel, Schema enforcement, and efficient querying.

    Easy to update/merge for future data.

Consumption:

    Data Analysts can query Delta Tables using SQL in Databricks.

    Data can be exposed to Power BI or other tools via ODBC/JDBC.

🧱 Architecture Diagram (Text-based)
    scss
    Copy
    Edit
    [NYC Taxi API]
      ↓ (ADF)
    [Bronze Layer - Raw Parquet in ADLS Gen2]
      ↓ (Databricks)
    [Silver Layer - Cleaned Parquet in ADLS Gen2]
      ↓ (Databricks SQL / Delta)
    [Gold Layer - Delta Tables in Databricks DB]
      ↓
    [Data Analysts, BI Tools (Power BI, etc.)]


🛡️ Security
      Authentication to API via Azure Entra ID (App Registration).

      Storage secured using RBAC and Storage Firewall.

      Databricks access controlled through AAD users/groups.
    

📈 Scalability & Optimization
    Partitioning and Z-Ordering on Delta Tables for performance.

    Auto-Loader in Databricks for continuous ingestion (optional upgrade).

    CI/CD can be introduced using Azure DevOps pipelines to deploy notebooks, ADF pipelines, and infrastructure as code (ARM/Bicep/Terraform).


✅ Summary
    You implemented a complete data pipeline on Azure with:

    API-based ingestion,

    Data Lake for scalable storage,

    Structured layers (Bronze → Silver → Gold),

    Delta Lake for performance and reliability,

    Security via Entra ID,

    Data ready for analysis.









