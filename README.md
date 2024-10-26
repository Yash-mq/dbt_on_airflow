# Data Pipeline Project

This project demonstrates a data pipeline using **dbt** integrated with **Apache Airflow** (via **Astronomer**) to manage, schedule, and monitor data transformations on **Snowflake**. The pipeline is Dockerized for easier deployment and configuration.

## Table of Contents

- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Configuration](#configuration)
- [Running the Project](#running-the-project)
- [Project Details](#project-details)
- [Troubleshooting](#troubleshooting)

## Project Structure

```plaintext
dbt-dag/
├── dags/                   # Airflow DAGs directory
│   ├── dbt_dag.py          # Main DAG to run dbt models and tests
│   └── dbt                 # dbt project directory
│       ├── data_pipeline   # dbt models and configuration
│       ├── generic.yml     # dbt model-level tests
│       ├── tpch_source.yml # dbt source definitions and tests
│       └── dbt_project.yml # Main dbt project configuration
├── Dockerfile              # Dockerfile for Airflow environment with dbt
├── docker-compose.override.yml # Custom docker-compose configuration for volume mounts and ports
├── .env                    # Environment file to set dbt project path
└── README.md               # Project README (this file)
