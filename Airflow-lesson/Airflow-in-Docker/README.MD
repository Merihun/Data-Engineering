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

a. by running CLI commands.
b. via a browser using the web interface.
c. using the REST API.

a. Running the CLI commands.
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
You can also use bash as parameter to enter interactive bash shell in the container or python to enter python container.
```
./airflow.sh bash
```
```
./airflow.sh python
  
```
 b. Accessing the web interface
 
Once the cluster has started up, you can log in to the web interface and begin experimenting with DAGs.

The webserver is available at: http://localhost:8080. The default account has the login airflow and the password airflow.

  c. Sending requests to the REST API
  
Basic username password authentication is currently supported for the REST API, which means you can use common tools to send requests to the API.

The webserver is available at: http://localhost:8080. The default account has the login airflow and the password airflow.

Here is a sample curl command, which sends a request to retrieve a pool list:
  
```
ENDPOINT_URL="http://localhost:8080/"
curl -X GET  \
    --user "airflow:airflow" \
    "${ENDPOINT_URL}/api/v1/pools"
```
#### Using custom images
- The most common scenarios where you want to build your own image are adding a new apt package, adding a new PyPI dependency (either individually or via requirements.txt) and embedding DAGs into the image.
- When you want to run Airflow locally, you might want to use an extended image, containing some additional dependencies - for example you might add new python packages, or upgrade airflow providers to a later version. 
- This can be done very easily by specifying build: . in your docker-compose.yaml and placing a custom Dockerfile alongside your docker-compose.yaml. 
- Then you can use docker-compose build command to build your image (you need to do it only once). 
- You can also add the --build flag to your docker-compose commands to rebuild the images on-the-fly when you run other docker-compose commands.
  
Example 1: Dockerfile for adding new apt package:
- The following example adds vim to the Airflow image. 
- When adding packages via apt you should switch to the root user when running the apt commands, but do not forget to switch back to the airflow user after installation is complete.
```
  
FROM apache/airflow:2.4.1
USER root
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
         vim \
  && apt-get autoremove -yqq --purge \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
USER airflow
  
```
Example 2: Dockerfile for adding new PyPI packages individually:  

- The following example adds lxml python package from PyPI to the image. 
- When adding packages via pip you need to use the airflow user rather than root. 
- Attempts to install pip packages as root will fail with an appropriate error message.
  
```
FROM apache/airflow:2.4.1
RUN pip install --no-cache-dir lxml
```
Example 3: Dockerfile for adding packages from requirements.txt
- The following example adds few python packages from requirements.txt from PyPI to the image.   
- When adding packages via pip you need to use the airflow user rather than root. 
- Attempts to install pip packages as root will fail with an appropriate error message.
```
FROM apache/airflow:2.4.1
COPY requirements.txt /
RUN pip install --no-cache-dir -r /requirements.txt
```
#### Embedding DAGs
Dockerfile
```
FROM apache/airflow:2.4.1

COPY --chown=airflow:root test_dag.py /opt/airflow/dags
```
test_dag.py
```python
import datetime

import pendulum

from airflow.models.dag import DAG
from airflow.operators.empty import EmptyOperator

now = pendulum.now(tz="UTC")
now_to_the_hour = (now - datetime.timedelta(0, 0, 0, 0, 0, 3)).replace(minute=0, second=0, microsecond=0)
START_DATE = now_to_the_hour
DAG_NAME = 'test_dag_v1'

dag = DAG(
    DAG_NAME,
    schedule='*/10 * * * *',
    default_args={'depends_on_past': True},
    start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
    catchup=False,
)

run_this_1 = EmptyOperator(task_id='run_this_1', dag=dag)
run_this_2 = EmptyOperator(task_id='run_this_2', dag=dag)
run_this_2.set_upstream(run_this_1)
run_this_3 = EmptyOperator(task_id='run_this_3', dag=dag)
run_this_3.set_upstream(run_this_2)
```
