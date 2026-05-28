# Incremental File Ingestion Framework using Microsoft Fabric

## Project Overview
This project demonstrates a metadata-driven incremental ETL pipeline built using Microsoft Fabric.  
The solution automates file ingestion from ADLS Gen2 into both a Lakehouse and a Dedicated SQL Warehouse while preventing duplicate file loads using a tracking mechanism.

The pipeline follows enterprise-grade orchestration concepts including:
- Incremental loading
- Metadata-driven processing
- File tracking and auditing
- Dynamic file handling
- Lakehouse + Warehouse integration

---

# Architecture

## Source
- Azure Data Lake Storage Gen2 (ADLS Gen2)
- CSV files

## Technologies Used
- Microsoft Fabric Pipelines
- Fabric Lakehouse
- Fabric Dedicated Warehouse
- SQL
- ADLS Gen2
- Copy Activity
- Lookup Activity
- Filter Activity
- ForEach Activity
- Script Activity

---

# Pipeline Workflow

## Step 1 — Get Metadata
The pipeline scans the source ADLS Gen2 folder and retrieves all available files dynamically.

## Step 2 — Lookup Existing Files
A Lookup activity checks previously loaded files stored inside the `FileTracker` table.

## Step 3 — Filter New Files
The pipeline compares source files against tracked files and keeps only new files for processing.

## Step 4 — ForEach Loop
The pipeline loops through every new file dynamically.

## Step 5 — Copy Raw Files to Lakehouse
Raw files are copied into the Fabric Lakehouse for archival and raw-layer storage.

## Step 6 — Load Data into Dedicated Warehouse
The processed file data is inserted into the `DimProduct` table inside the Fabric Dedicated SQL Warehouse.

## Step 7 — Update Tracking Table
The pipeline inserts the processed filename and load timestamp into the `FileTracker` table to prevent duplicate ingestion in future runs.

---

# Database Objects

## DimProduct
Stores ingested product data.

```sql
CREATE TABLE DimProduct (
    ProductID INT,
    ProductName VARCHAR(100),
    Category VARCHAR(50),
    Price FLOAT,
    FileName VARCHAR(100),
    LoadDate DATETIME2(0)
);
```

## FileTracker
Tracks already processed files.

```sql
CREATE TABLE FileTracker (
    FileName VARCHAR(100),
    LoadDate DATETIME2(0)
);
```

---

# Key Features

- Incremental Data Loading
- Duplicate Prevention Logic
- Metadata-Driven Architecture
- Dynamic File Processing
- Enterprise ETL Design
- Fabric Lakehouse Integration
- Dedicated Warehouse Loading
- File Auditing & Tracking

---

# Business Value

This framework ensures:
- Faster ingestion processes
- Reduced duplicate data loads
- Automated file orchestration
- Better scalability for enterprise workloads
- Reliable audit tracking for data operations

---

# Future Improvements

Potential enhancements include:
- Schema drift handling
- Dynamic table creation
- SCD Type 1 / Type 2 implementation
- Parquet/Delta optimization
- Error handling and alerting
- Notebook integration for advanced transformations

---

# Project Outcome

This project demonstrates practical enterprise-level Data Engineering concepts using Microsoft Fabric and modern ETL orchestration techniques.
