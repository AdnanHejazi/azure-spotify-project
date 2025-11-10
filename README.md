# Modern Data Lakehouse Pipeline using Azure Data Factory, Databricks & LakeFlow

## 🚀 Project Overview
This project demonstrates an **end-to-end Azure Data Lakehouse pipeline** built using **Azure Data Factory**, **Azure Databricks**, **Azure Data Lake Storage Gen2**, and **Declarative LakeFlow**.  
The goal is to design an **incremental ingestion and transformation framework** that moves data from **Azure SQL Database** into a **multi-layered Data Lake (Bronze → Silver → Gold)** architecture for downstream analytics.

---

## 🧱 Architecture Diagram
![Architecture Diagram] (./Screeenshots/architecture.png)

### 🔄 **Data Flow Summary**
1. **Azure SQL Database (Source)**  
   - Acts as the transactional system storing source data.  
   - Data is extracted incrementally based on a watermark column.

2. **Azure Data Factory (ADF)**  
   - Orchestrates data ingestion pipelines.  
   - Performs **incremental load** from Azure SQL Database into **Azure Data Lake (Bronze Layer)**.  
   - Data is stored in **Parquet format** for efficient storage and query performance.

3. **Azure Data Lake Storage Gen2 (ADLS)**  
   - Centralized storage following a multi-zone architecture:  
     - **Bronze** → Raw ingested data  
     - **Silver** → Cleaned and transformed data  
     - **Gold** → Business-ready, aggregated data  

4. **Azure Databricks (Serverless)**  
   - Processes data from the **Bronze layer** using **PySpark**.  
   - Applies transformations, schema standardization, and data cleansing.  
   - Stores the output in **Silver layer** in **Delta format** for ACID transactions and time travel.

5. **Declarative LakeFlow Pipeline**  
   - Performs **data quality checks** (null validation, duplicate checks, referential integrity).  
   - Implements **Slowly Changing Dimension (SCD) Type 2** logic to maintain historical versions of records.  
   - Writes the final curated dataset to the **Gold layer**, ready for analytics and reporting.

---

## 🔧 **Pipeline Workflow**
### 1️⃣ **Ingestion – Azure Data Factory**
- Built a pipeline to extract data incrementally from Azure SQL Database.  
- Used lookup and watermark techniques to identify new or changed records.  
- Landed the data in **Bronze zone** of ADLS in **Parquet format**.
![Azure Data Factory Diagram] (./Screeenshots/adf.png)

### 2️⃣ **Transformation – Azure Databricks**
- Connected Databricks to ADLS using secure access keys / managed identity.  
- Read the **Parquet files** from Bronze, applied PySpark transformations:
  - Data type standardization  
  - Deduplication and null handling  
  - Business rule applications  
- Wrote the cleansed data into **Silver layer** in **Delta format**.
![Azure Databricks Diagram] (./Screeenshots/Silver_pipeline.png)

### 3️⃣ **Quality & SCD Management – LakeFlow**
- Used **Declarative LakeFlow pipeline** to:
  - Validate data quality across key dimensions.  
  - Apply **SCD Type 2 logic** to track historical changes in dimensional data.  
  - Write the final datasets to the **Gold layer** in Delta format.
![Gold-Layer] (./Screeenshots/gold_dlt.png)

### 4️⃣ **Downstream Analytics**
- Gold layer data can be consumed by **Power BI**, **Synapse Serverless**, or **Databricks SQL** for BI and reporting.

---

## 📊 **Future Enhancements**
- Integrate **Power BI** dashboards from the Gold layer.  
- Add **data validation alerts** using Azure Logic Apps or Event Grid.  
- Extend LakeFlow logic to handle multi-table dependencies.  
- Automate full CICD using **GitHub Actions** and **ADF triggers**.
