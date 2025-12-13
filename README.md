# Distributed Machine Learning with Apache Spark (MLlib)

This project demonstrates how to build and run a **distributed machine learning pipeline using Apache Spark MLlib**, deployed on a **multi-node Spark cluster** with Docker.

The focus is on understanding **distributed data processing, ML pipelines, and cluster-level execution**, rather than just model accuracy.

---

## Project Overview

In this project, I:

- Built a Spark cluster (1 Master + 2 Workers) using **Docker Compose**
- Used **PySpark and Spark MLlib** to train classification models
- Worked with a real-world dataset on **AI’s impact on jobs by 2030**
- Trained:
  - A **global model** using the full dataset
  - **Per-node models** using data partitions assigned to individual nodes
- Monitored execution using the **Spark Web UI**

---

## Dataset

**AI Impact on Jobs 2030** (Kaggle)

The dataset contains structured information about job roles and their exposure to AI and automation.

### Key Features
- Job_Title (categorical)
- Education_Level (categorical)
- Average_Salary
- Years_Experience
- AI_Exposure_Index
- Automation_Probability_2030
- Tech_Growth_Factor
- Skill_1 … Skill_10

### Target Variable
- **Risk_Category** — categorical label indicating job risk level due to AI

---

## Architecture

- **Spark Master**
- **Two Spark Workers**
- **JupyterLab (PySpark)**
- Shared volume mounted at `/data` across all containers

The dataset was manually split into:
- `jobs_master.csv`
- `jobs_worker1.csv`
- `jobs_worker2.csv`

This simulates distributed data ownership across nodes.

---

## Machine Learning Pipeline

Implemented using **Spark MLlib**:

1. StringIndexer for labels and categorical features
2. OneHotEncoder for categorical variables
3. VectorAssembler for feature construction
4. Logistic Regression classifier
5. Multiclass F1-score for evaluation

The same pipeline was reused for:
- Global model training
- Per-node model training (fair comparison)

---

## Spark UI Evidence

Below is a screenshot from the **Spark Web UI (Executors page)** captured during model training:

![Spark Executors UI](https://github.com/hakim733/Spark-Mllib-AI-Impact/blob/main/Screenshot%202025-12-13%20at%2011.16.10.png)

This screenshot shows:
- Multiple active executors
- Tasks distributed across worker nodes
- Shuffle read/write activity
- No failed tasks

This confirms **true distributed execution**, not local mode.

---

## Results Summary

- The **global model** trained on all data showed more stable performance
- **Per-node models** trained faster but exhibited more variance due to smaller datasets
- Spark efficiently distributed tasks across executors and reused cached results

---

## Technologies Used

- Apache Spark 3.x
- Spark MLlib
- PySpark
- Docker & Docker Compose
- JupyterLab
- Python 3

---

## Key Takeaways

- Spark MLlib enables scalable ML pipelines with minimal code changes
- Distributed training improves generalization when data is partitioned
- Spark Web UI is essential for understanding execution behavior and performance
- Docker simplifies reproducible big data environments

