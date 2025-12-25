Distributed Machine Learning with Apache Spark (MLlib)

This project demonstrates end-to-end distributed machine learning using Apache Spark MLlib, executed on a multi-node Spark cluster deployed with Docker.

The emphasis is on cluster architecture, distributed data processing, and scalable ML pipelines, rather than isolated model performance.

Why This Project Matters

Many Spark projects stop at local mode or single-node execution.
This project goes further by:

Running Spark in true multi-worker cluster mode

Manually partitioning data to simulate data locality

Comparing per-node vs distributed ML training

Verifying execution using Spark Web UI metrics

This reflects real-world big data and ML engineering workflows.

Problem Statement

AI and automation are expected to significantly impact job markets by 2030.
The objective is to predict job risk levels using structured job attributes and to evaluate how distributed systems improve scalability and consistency.

Dataset

AI Impact on Jobs 2030 (Kaggle)

The dataset contains real, structured records describing job roles and their exposure to AI.

Features

Job title

Education level

Average salary

Years of experience

AI exposure index

Automation probability (2030)

Technology growth factor

Skill indicators (Skill_1 … Skill_10)

Target

Risk_Category — categorical job risk level due to AI and automation

✔ Real dataset
✔ No synthetic data

System Architecture

The system is deployed using Docker Compose and consists of:

Spark Master (Driver coordination)

Two Spark Workers (Executors)

JupyterLab (PySpark driver interface)

Shared volume (/data) across all containers

JupyterLab (Driver)
        ↓
   Spark Master
        ↓
--------------------
| Worker 1 | Worker 2 |
--------------------


Spark executes jobs in parallel across workers using executors.

Data Partitioning Strategy

To simulate distributed data ownership:

The dataset was shuffled and split into:

jobs_master.csv

jobs_worker1.csv

jobs_worker2.csv

Each file is loaded independently, then unified into a global DataFrame for distributed processing.

This approach demonstrates:

Data locality

Parallel ingestion

Spark’s ability to coordinate distributed datasets

Machine Learning Pipeline (Spark MLlib)

A reusable Spark ML pipeline was built using:

StringIndexer – label & categorical encoding

OneHotEncoder – sparse categorical vectors

VectorAssembler – feature vector construction

LogisticRegression – distributed classifier

The same pipeline was reused for all experiments, ensuring fair comparison.

Experiments
1. Global Distributed Model

Trained on the full dataset

Executed across all Spark workers

Benefits from broader data coverage and parallelism

2. Per-Node Models

Each node trains a model on its local data split

Faster training due to smaller data

Higher variance in performance

Results Summary
Training Strategy	Dataset Size	F1 Score	Training Time
Per-node models	~1,000 rows	~0.99	Faster locally
Distributed model	~3,000 rows	0.995	Efficient via parallelism

Key Insight:
Distributed training provides stable accuracy while maintaining competitive training time due to Spark’s parallel execution.

Spark UI Validation

The Spark Web UI confirms distributed execution:

Multiple active executors

Over 1,200 completed tasks

Balanced task scheduling across workers

Shuffle read/write operations

Zero failed tasks

This confirms the workload ran across the cluster, not locally.

Technologies Used

Apache Spark 3.x

Spark MLlib

PySpark

Docker & Docker Compose

JupyterLab

Python
