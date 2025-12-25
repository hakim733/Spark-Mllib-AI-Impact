# Distributed Machine Learning with Apache Spark (MLlib)

This project demonstrates **end-to-end distributed machine learning** using **Apache Spark MLlib**, executed on a **multi-node Spark cluster** deployed with Docker.

The focus is on **distributed data processing, scalable ML pipelines, and cluster-level execution**, not on running Spark in local mode.

---

## Why This Project Matters

Many Spark projects stop at local or single-node execution.  
This project demonstrates **true distributed computing** by:

- Running Spark in **multi-worker cluster mode**
- Manually partitioning data to simulate **data locality**
- Comparing **per-node vs distributed ML training**
- Verifying execution using the **Spark Web UI**

This closely mirrors real-world **data engineering and ML platform workflows**.

---

## Problem Statement

AI and automation are expected to significantly impact job markets by 2030.  
The objective is to **predict job risk levels** using structured job attributes and to analyze how distributed systems improve scalability and consistency.

---

## Dataset

**AI Impact on Jobs 2030 (Kaggle)**

A real-world dataset containing structured information about job roles and their exposure to AI.

### Features
- Job_Title
- Education_Level
- Average_Salary
- Years_Experience
- AI_Exposure_Index
- Automation_Probability_2030
- Tech_Growth_Factor
- Skill_1 … Skill_10

### Target Variable
- `Risk_Category` — categorical job risk level due to AI and automation

✔ Real dataset  
✔ No synthetic data  

---

## System Architecture

The system is deployed using **Docker Compose** and consists of:

- **Spark Master** (driver coordination)
- **Two Spark Workers** (executors)
- **JupyterLab** (PySpark driver interface)
- **Shared data volume** mounted at `/data`

