# Airflow + dbt + Snowflake (with Cosmos)

This project demonstrates how to orchestrate a **dbt project on Snowflake** using **Apache Airflow** and the [Astronomer Cosmos](https://github.com/astronomer/astronomer-cosmos) library.  

It includes:
- A complete dbt project (`dags/data_pipeline/`) with models, macros, and tests
- An Airflow DAG (`dags/dbt_dag.py`) that runs dbt commands using Cosmos
- Dockerized setup with Astro CLI for local development

### 1. Clone the repo
```bash
git clone https://github.com/<your-username>/dbt-dag.git
cd dbt-dag
```

### 2. Configure environment variables
cp .env.example .env
- Edit .env with actual values

### 3. Start Airflow locally using Astro CLI
astro dev start
The Airflow UI will be available at: http://localhost:8080
Default credentials:
User: admin
Password: admin

### 4. Create the Snowflake connection in Airflow

This project expects an Airflow connection named snowflake_conn.
Run this once (from your host machine) to register it inside the scheduler container:
astro dev bash scheduler -c \
'airflow connections add snowflake_conn \
 --conn-uri="snowflake://${SNOWFLAKE_USER}:${SNOWFLAKE_PASSWORD}@${SNOWFLAKE_ACCOUNT}/${SNOWFLAKE_DATABASE}/${SNOWFLAKE_SCHEMA}?warehouse=${SNOWFLAKE_WAREHOUSE}&role=${SNOWFLAKE_ROLE}"'
 
 ### 5. Trigger the dbt DAG
 In the Airflow UI:
	1. Navigate to DAGs
	2. Enable dbt_dag
	3. Trigger it manually or wait for its daily schedule
	
### Requirements

Defined in requirements.txt:
astronomer-cosmos>=1.10
dbt-core==1.10.10
dbt-snowflake==1.10.0
apache-airflow-providers-snowflake


