### Running Airflow in Docker

Quick-start guide that will allow you to quickly get Airflow up and running with __CeleryExecutor__ in Docker.

1. Install __Docker Community Edition (CE__ on your workstation.
2. Install __Docker Compose__ v1.29.1 or newer on your workstation.

You should allocate at least 4GB memory for the Docker Engine (ideally 8GB).

You can check if you have enough memory by running this command:
```bash
docker run --rm "debian:bullseye-slim" bash -c 'numfmt --to iec $(echo $(($(getconf _PHYS_PAGES) * $(getconf PAGE_SIZE))))'
```
