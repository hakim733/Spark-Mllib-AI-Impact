Distributed Machine Learning with Apache Spark (MLlib)

This project demonstrates how to design, execute, and evaluate a distributed machine learning pipeline using Apache Spark MLlib, deployed on a multi-node Spark cluster orchestrated with Docker Compose.

The work focuses on distributed data processing, Spark ML pipelines, resource utilization, and performance comparison between per-node training and globally distributed training.

Project Overview

In this project, I:

Built a Spark standalone cluster (1 Master + 2 Workers) using Docker Compose.

Used PySpark and Spark MLlib to implement a full classification pipeline.

Worked with a real-world dataset on AI’s impact on jobs by 2030.

Trained and compared:

A global distributed model using the unified dataset.

Per-node models trained on individual data partitions.

Analyzed execution using the Spark Web UI.

Compared training time, F1-score, and resource utilization across approaches.

This setup ensures true distributed execution rather than local-mode Spark.

Dataset

AI Impact on Jobs 2030
Source: Kaggle
https://www.kaggle.com/datasets/khushikyad001/ai-impact-on-jobs-2030

The dataset contains structured information describing job roles and their exposure to AI and automation.

Key Features

Job_Title (categorical)

Education_Level (categorical)

Average_Salary

Years_Experience

AI_Exposure_Index

Automation_Probability_2030

Tech_Growth_Factor

Skill_1 … Skill_10

Target Variable

Risk_Category — categorical label representing job risk due to AI and automation

All analyses are performed on real data; no synthetic data generation was used.

Architecture

The system consists of:

Spark Master (cluster coordinator)

Two Spark Worker nodes (executors)

JupyterLab (PySpark) acting as the Spark Driver

Shared Docker volume mounted at /data in all containers

Cluster Diagram
        +----------------+
        |  Spark Master  |
        |  (Scheduler)   |
        +--------+-------+
                 |
      ---------------------------
      |                         |
+--------------+        +--------------+
|  Worker 1    |        |  Worker 2    |
|  Executor    |        |  Executor    |
+--------------+        +--------------+

Data Sharing Strategy

Dataset stored in a shared Docker volume (/data)

Data manually partitioned into:

jobs_master.csv

jobs_worker1.csv

jobs_worker2.csv

Spark unifies these files into a distributed DataFrame

This design simulates distributed data ownership while enabling parallel execution.

Step-by-Step Execution
Step 1: Cluster Setup and Spark Session

Docker Compose launches Spark Master and Workers.

JupyterLab connects to the cluster using:

spark://bd-spark-master:7077


Spark UI confirms multiple active executors.

Observation:
The Spark session runs in cluster mode, not local mode.

Step 2: Manual Data Splitting

Dataset shuffled and split into three CSV files.

Each split contains approximately one-third of the records.

Observation:
Manual splitting makes data distribution explicit and auditable.

Step 3: Loading Data on Each Node

Each CSV loaded as a Spark DataFrame.

DataFrames unioned into a unified distributed dataset.

Observation:
Spark reads files in parallel and distributes partitions across executors.

Step 4: Spark SQL Processing

Schema validation and exploratory queries using Spark SQL.

Dataset repartitioned by label for improved parallelism.

Data cached to optimize repeated access.

Observation:
Spark DAG optimization and caching reduce redundant computation.

Step 5: Machine Learning Pipeline

A reusable Spark MLlib pipeline was implemented:

StringIndexer for labels and categorical features

OneHotEncoder for categorical encoding

VectorAssembler for feature construction

LogisticRegression classifier

The same pipeline is used for all experiments to ensure fair comparison.

Part 1 — Global Distributed Model

Model trained on the unified dataset

Training distributed across all worker nodes

Results

F1-score: 0.995

Training time: ~7.6 seconds

Observation:
Access to the full dataset provides stable performance and effective generalization.

Part 2 — Per-Node Models

Separate models were trained on each data partition.

Per-Node Results
Node	Records	F1 Score	Training Time (s)
Master	1006	0.9911	9.43
Worker 1	965	0.9979	8.77
Worker 2	1029	0.9951	6.48

Observation:
Smaller datasets train faster but show more variance in performance.

Global vs Per-Node Comparison
Model Type	Records	F1 Score	Training Time (s)
Per-node (avg)	~1000	~0.995	~8.2
Distributed Global	3000	0.9950	7.61

Conclusion:
Distributed training combines scalability with stable accuracy.

Spark UI Monitoring

Spark Web UI confirms distributed execution:

3 active executors

1,276 completed tasks

Balanced task distribution

Shuffle read/write operations

Zero failed tasks

Screenshots are included in the screenshots/ directory.

Project Structure
spark-project/
├── docker-compose.yml
├── spark_data/
│   ├── AI_Impact_on_Jobs_2030.csv
│   └── split/
│       ├── jobs_master.csv
│       ├── jobs_worker1.csv
│       └── jobs_worker2.csv
├── notebook.ipynb
├── screenshots/
│   └── spark_ui_executors.png
└── README.md

Technologies Used

Apache Spark 3.x

Spark MLlib

PySpark

Docker & Docker Compose

JupyterLab

Python 3

Key Takeaways

Spark MLlib enables scalable ML with minimal overhead

Manual data partitioning clarifies distributed ownership

Global distributed training provides stable performance

Spark UI validates real parallel execution

Docker enables reproducible big data environments
