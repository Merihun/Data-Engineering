### Running Airflow in Docker

- This Quick-start guide is a step-by-step guide that can help you get Apache Airflow v2.3.0 up and running on your local machine via Docker.
- It will allow you to quickly get Airflow up and running with __CeleryExecutor__ in Docker.
- Note that the Airflow Docker images should only be used for testing purposes. 
- If you are planning to deploy Airflow on production environments I’d recommend running it on Kubernetes with the official helm chart.

1. Install __Docker Community Edition (CE__ on your workstation.
2. Install __Docker Compose__ v1.29.1 or newer on your workstation.

You should allocate at least 4GB memory for the Docker Engine (ideally 8GB).

You can check if you have enough memory by running this command:
```bash
docker run --rm "debian:bullseye-slim" bash -c 'numfmt --to iec $(echo $(($(getconf _PHYS_PAGES) * $(getconf PAGE_SIZE))))'
```

#### Step 1: Fetching docker-compose.yaml

Create a new directory on your home directory (let’s call it airflow-local):
```bash
$ mkdir airflow-local
$ cd airflow-local
```

Fetch docker-compose.yaml.

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
#### Step 2: Create directories
create three additional directories:
* dags
* logs
* plugins

```bash
$ mkdir -p ./dags ./logs ./plugins
```

#### Step 3: Setting the Airflow user
- Export an environment variable to ensure that the folder on your host machine and the folders within the containers share the same permissions. 
- We will simply add these variables into a file called .env.
```bash
$ echo -e "AIRFLOW_UID=$(id -u)\nAIRFLOW_GID=0" > .env
```
#### Step 4: Initialise the Airflow Database
- Initialise the Airflow Database by first starting the airflow-init container
- This service will essentially run ```airflow db init``` and create the admin user for the Airflow Database.
- By default, the account created has the login __airflow__ and the password __airflow__.
```bash
$ docker-compose up airflow-init
```
#### Step 5: Start Airflow services

```bash
$ docker-compose up
```
- The above command may take a while since multiple services need to be started.
- Verify that these images are up and running using ``` $ docker ps ```

#### Step 6: Access Airflow UI

Open localhost:8080 on local browser. Both username and password are ``` airflow ```.

#### Step 7: Enter the Airflow Worker container
- You can even enter the worker container so that you can run airflow commands using ``` docker exec -it <container-id> bash ```  command. 
- You can find <container-id> for the Airflow worker service by running ``` docker ps ```

#### Step 8: Cleaning up the mess
  ```
  $ docker-compose down --volumes --rmi all
  
  ```
  This command will stop and delete all running containers, delete volumes with database data and downloaded images.
  
 #### Step 9: Accessing the environment
  After starting Airflow, you can interact with it in 3 ways:

* by running CLI commands.
* via a browser using the web interface.
* using the REST API.

Running the CLI commands.
You can also run CLI commands, but you have to do it in one of the defined airflow-* services. For example, to run airflow info, run the following command:
```
docker-compose run airflow-worker airflow info
```
If you have Linux or Mac OS, you can make your work easier and download a optional wrapper scripts that will allow you to run commands with a simpler command.
```
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.4.1/airflow.sh'
chmod +x airflow.sh
```
Now you can run commands easier.
```
  ./airflow.sh info
```
