### Running Airflow in Docker

Quick-start guide that will allow you to quickly get Airflow up and running with __CeleryExecutor__ in Docker.

1. Install __Docker Community Edition (CE__ on your workstation.
2. Install __Docker Compose__ v1.29.1 or newer on your workstation.

You should allocate at least 4GB memory for the Docker Engine (ideally 8GB).

You can check if you have enough memory by running this command:
```bash
docker run --rm "debian:bullseye-slim" bash -c 'numfmt --to iec $(echo $(($(getconf _PHYS_PAGES) * $(getconf PAGE_SIZE))))'
```

#### Fetching docker-compose.yaml

To deploy Airflow on Docker Compose, you should fetch __docker-compose.yaml__.
```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.4.1/docker-compose.yaml'
```

This file contains several service definitions:
```text
airflow-scheduler - The scheduler monitors all tasks and DAGs, then triggers the task instances once their dependencies are complete.
airflow-webserver - The webserver is available at http://localhost:8080.
airflow-worker - The worker that executes the tasks given by the scheduler.
airflow-init - The initialization service.
postgres - The database.
redis - The redis - broker that forwards messages from scheduler to worker.
```
