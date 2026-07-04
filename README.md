 Patient Health Monitoring Pipeline Using AWS
PROJECT TITLE
Patient Health Monitoring Data Pipeline
BUSINESS SCENARIO
A hospital receives patient health monitoring files every hour from:
- ICU monitoring systems
- Wearable health devices
- Hospital management systems
- Emergency care systems
The hospital wants to:
- Store incoming patient data securely
- Validate health records automatically
- Detect corrupted files
- Clean and transform patient data
- Catalog metadata automatically
- Store processed healthcare data
- Separate bad patient records
- Maintain production-ready ETL workflows
PROJECT ARCHITECTURE
PHASE 1 — PROJECT DEFINITION
Objective
Build a healthcare ETL pipeline that processes patient monitoring data securely.
PROJECT GOALS
build a system that:
- Ingests patient data
- Validates incoming files
- Cleans medical records
- Detects invalid patient readings
- Stores transformed healthcare data
- Handles schema changes
HEALTH METRICS TO TRACK
| Metric | Example |
| ---------------- | ------- |
| Heart Rate | 72 BPM |
| Blood Pressure | 120/80 |
| Oxygen Level | 98% |
| Temperature | 98.6 F |
| Respiration Rate | 16/min |
PHASE 2 — DATA COLLECTION & STORAGE
TASK 1 — Create S3 Bucket
Bucket Name
patient-health-monitoring
Create Folder Structure
raw/vitals/
raw/patients/
raw/devices/
processed/vitals/
processed/patients/
processed/reports/
rejected/
PHASE 3 — IAM IMPLEMENTATION
TASK 1 — Glue IAM Role
Permissions
* Read raw bucket
* Write processed bucket
* Access Glue Catalog
* Access logs
TASK 2 — Lambda IAM Role
Permissions
* Read uploaded files
* Move rejected files
* Trigger crawler
* Trigger ETL job
PHASE 4 — SAMPLE HEALTHCARE DATA
FILE 1 — patient_vitals.csv
patient_id,timestamp,heart_rate,blood_pressure,oxygen_level,temperature
P1001,2025-01-10 08:00:00,72,120/80,98,98.6
P1002,2025-01-10 08:05:00,95,140/90,96,99.1
P1003,2025-01-10 08:10:00,45,90/60,88,101.2
P1004,2025-01-10 08:15:00,,130/85,97,98.4
P1005,2025-01-10 08:20:00,110,150/95,,100.1
P1005,2025-01-10 08:20:00,110,150/95,,100.1
FILE 2 — patient_details.csv
patient_id,name,age,gender,city,disease
P1001,Rahul Sharma,45,Male,Delhi,Diabetes
P1002,Priya Verma,52,Female,Mumbai,Hypertension
P1003,Amit Singh,60,Male,Pune,Heart Disease
P1004,Neha Kapoor,29,Female,Delhi,Asthma
P1005,Rohit Jain,48,Male,Jaipur,Diabetes
FILE 3 — device_logs.csv
device_id,patient_id,device_type,status,last_sync
D101,P1001,SmartWatch,Active,2025-01-10 08:00:00
D102,P1002,Oximeter,Inactive,2025-01-10 07:50:00
D103,P1003,HeartMonitor,Active,2025-01-10 08:05:00
# REAL-WORLD DATA PROBLEMS INCLUDED
Students must detect:
| Issue | Example |
| ------------------------ | ------------------ |
| Null values | heart_rate missing |
| Duplicate records | P1005 repeated |
| Critical health readings | oxygen_level = 88 |
| Missing patient metrics | oxygen_level blank |
| Schema changes | new column added |
PHASE 5 — GLUE CRAWLER
Objective
Automatically discover healthcare schema.
TASKS
Students must:
* Create crawler
* Scan raw healthcare bucket
* Create Glue database: healthcare_db
Expected Tables
| Table | Source |
| --------------- | ------------ |
| patient_vitals | raw/vitals |
| patient_details | raw/patients |
| device_logs | raw/devices |
PHASE 6 — GLUE DATA CATALOG
Objective
Store metadata automatically.
Catalog contains:
* Column names
* Data types
* Table locations
PHASE 7 — GLUE ETL JOB
MAIN HEALTHCARE ETL PIPELINE
Students must create Glue ETL using PySpark.
ETL REQUIREMENTS
Step 1 — Read Raw Files
Read:
* patient_vitals.csv
* patient_details.csv
* device_logs.csv
Step 2 — Data Cleaning
Handle:
* Null readings
* Duplicate records
* Incorrect values
* Missing patient information
Step 3 — Data Transformation
Create:
* health_status
* alert_flag
* monitoring_date
* monitoring_hour
Example Rules
| Condition | Status |
| ----------------- | --------- |
| oxygen_level < 90 | Critical |
| temperature > 100 | Fever |
| heart_rate > 100 | High Risk |
Step 4 — Join Datasets
Join:
* Patient vitals
* Patient details
* Device logs
Final Output Columns
| Column |
| --------------- |
| patient_id |
| patient_name |
| disease |
| heart_rate |
| oxygen_level |
| temperature |
| health_status |
| device_type |
| monitoring_date |
Step 5 — Store Processed Data
Write transformed data into:
processed/reports/
Format:
* Parquet
PHASE 8 — EXPECTED OUTPUT SAMPLE
patient_id,patient_name,disease,heart_rate,oxygen_level,temperature,health_status
P1001,Rahul Sharma,Diabetes,72,98,98.6,Normal
P1002,Priya Verma,Hypertension,95,96,99.1,Warning
P1003,Amit Singh,Heart Disease,45,88,101.2,Critical
