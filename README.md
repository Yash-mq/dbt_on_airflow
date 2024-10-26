Data Pipeline Project
This project demonstrates a data pipeline using dbt integrated with Apache Airflow (via Astronomer) to manage, schedule, and monitor data transformations on Snowflake. The pipeline is Dockerized for easier deployment and configuration.

Table of Contents
Project Structure
Prerequisites
Setup Instructions
Configuration
Running the Project
Project Details
Troubleshooting
Project Structure
bash
Copy code
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
Prerequisites
Docker and Docker Compose installed on your machine.
Astronomer CLI installed for managing Airflow.
A Snowflake account and valid connection credentials.
Python and dbt installed locally (for development and testing dbt models).
Setup Instructions
Clone the Repository:

bash
Copy code
git clone <repository-url>
cd dbt-dag
Set up the .env File: Create an .env file in the root directory to specify the path to your dbt project:

plaintext
Copy code
AIRFLOW_DBT_PROJECT_PATH=/path/to/your/local/dbt_project
Replace /path/to/your/local/dbt_project with the actual path to your dbt project directory. This path will be mounted in the Airflow container.

Configure Airflow Connections: In Airflow, create a Snowflake connection:

Conn ID: snowflake_conn
Connection Type: Snowflake
Fill in the Snowflake connection details: account, user, password, warehouse, database, and schema.
Configuration
Airflow DAG (dbt_dag.py)
The DAG runs dbt models and tests on Snowflake:

DbtDag: Manages dbt commands (run and test) as Airflow tasks.
ProfileConfig: Configures the Snowflake connection, referring to Airflow’s snowflake_conn.
ExecutionConfig: Points to the dbt executable in the Docker environment.
dbt Project (dbt_project.yml)
Defines models, paths, and configuration for dbt, including:

Source definitions: Configured in tpch_source.yml with source-level tests.
Model-level tests: Configured in generic.yml, including tests for uniqueness, relationships, and accepted values.
Running the Project
Start Airflow and dbt Services:

bash
Copy code
astro dev start
Access Airflow UI: Open your browser and navigate to http://localhost:8080 to view the Airflow interface.

Trigger the DAG:

In the Airflow UI, find the dbt_dag DAG and trigger it.
Monitor the execution of dbt models and tests in the DAG view or task logs.
Project Details
dbt Model Details
Source Definitions (tpch_source.yml): Defines source tables (orders, lineitem) and their tests (e.g., unique, relationships).
Model-level Tests (generic.yml): Applies unique, not_null, and accepted_values tests to specific fields in fct_orders.
Model Configuration (dbt_project.yml): Organizes models in staging and marts, with models materialized as views.
Docker Configuration
Dockerfile: Builds a custom image with dbt-snowflake installed in a virtual environment (dbt_venv).
docker-compose.override.yml: Mounts the dbt project directory into the container using the environment variable AIRFLOW_DBT_PROJECT_PATH and configures PostgreSQL port mapping.
Troubleshooting
Port Conflicts: If you get errors about port 5432 being in use, adjust the docker-compose.override.yml to map to a different port, like 5433.
File Not Found Errors: Ensure paths in dbt_dag.py match the mounted paths in docker-compose.override.yml.
Dependencies Not Found: If dbt dependencies are missing, run dbt deps in the container or set operator_args={"install_deps": True} in dbt_dag.py.
