5# 📊 Dynamic Data Ingestion and Storage in HDFS with Automated Hive Integration

---

## 📌 Project Title
**Dynamic Data Ingestion and Storage in HDFS with Automated Hive Integration**

---

## 🎯 Objective
The objective of this project is to design and implement a system that:
- Dynamically ingests structured data from an external source.
- Stores it inside **Hadoop Distributed File System (HDFS)**.
- Integrates the data into **Hive** for structured querying and visualization.
- Supports **automation** using scripting for efficient data refreshes.

---

## 📝 Problem Statement
Organizations often deal with **large external datasets** that need to be integrated into big data ecosystems.  
The challenge is to:
1. Fetch the dataset from a public source.  
2. Store it in **HDFS** for scalability and durability.  
3. Define the correct schema and integrate with **Hive**.  
4. Enable **querying and analysis** using HiveQL.  
5. Automate the entire process for repeatability.  

This project demonstrates how to build such a pipeline using Hadoop + Hive.

---

## ⚙️ Platform
- **Virtual Hadoop Machine** running:
  - HDFS (Hadoop Distributed File System)  
  - Hive (SQL-like querying engine)  
- **Host Machine**: Used for downloading files if VM cannot download directly.  

---

## 📂 Dataset
We are using population estimate data from the **US Census Bureau**:  
🔗 [Census Data 2020–2023](https://www2.census.gov/programs-surveys/popest/datasets/2020-2023/cities/totals/sub-est2023.csv)

---

## 🚀 Project Workflow (Step by Step)

### **Step 2: Upload File to HDFS**
After downloading the dataset (`sub-est2023.csv`) into your VM:


hdfs dfs -mkdir -p /user/training/census
hdfs dfs -put -f sub-est2023.csv /user/training/census/

---

###  **Step 3: View File Content in HDFS**
Use the following command to confirm that the file has been uploaded correctly and to preview its contents:


hdfs dfs -cat /user/training/census/sub-est2023.csv | head -20


---





---

### **Step 4: Start Hive**
Open Hive from the terminal:


hive



---

### **Step 5: Create and Use a Database**
Before creating a Hive table, we need a dedicated database to organize our project data.  

Run the following commands inside the Hive shell:

-- Create a database (only if it doesn’t exist already)
CREATE DATABASE IF NOT EXISTS census_db;

-- Switch to the database
USE census_db;

-- Verify the current database
SELECT current_database();



---

### **Step 6: Create Hive Table**
Now, create a Hive table to store the census data.  
The schema should match the structure of the CSV file.  
For simplicity, we define all columns as `STRING` (you can later refine datatypes like INT or BIGINT).  


-- Create Hive table for census data
CREATE TABLE sub_est2023 (
  SUMLEV STRING,
  STATE STRING,
  COUNTY STRING,
  PLACE STRING,
  COUSUB STRING,
  CONCIT STRING,
  PRIMGEO_FLAG STRING,
  FUNCSTAT STRING,
  NAME STRING,
  STNAME STRING,
  CENSUS2020POP STRING,
  ESTIMATESBASE2020 STRING,
  POPESTIMATE2020 STRING,
  POPESTIMATE2021 STRING,
  POPESTIMATE2022 STRING,
  POPESTIMATE2023 STRING
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
  "separatorChar" = ",",
  "quoteChar"     = "\""
)
STORED AS TEXTFILE
TBLPROPERTIES ("skip.header.line.count"="1");



---

### **Step 7: Load Data into Hive and Verify**
Once the Hive table is created, load the CSV data from HDFS into the table.  

```sql
-- Load data from HDFS into the Hive table
LOAD DATA INPATH '/user/training/census/sub-est2023.csv'
INTO TABLE sub_est2023;




