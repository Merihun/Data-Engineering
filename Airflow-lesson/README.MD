# Apache AirFlow
Imagine the scenario where you have to run multiple daily jobs to extract data from a data lake, preprocess them, and store the cleaned datasets to a dedicated database. It would be extremely tedious if we have to run the pipeline everyday, constantly checking for possible errors. This is where Airflow comes in handy: it provides you with all the tools to build and monitor multiple data pipelines, automatically.

- Airflow is a platform created by the community to programmatically author, schedule and monitor workflows.
- AirFlow is a Workflow Scheduling and Monitoring Platform
- Airflow was started by Airbnb in 2014. In 2016 it became an Apache incubator and in 2019 it was adopted as an Apache software foundation project.
- It is a platform written in Python to schedule and monitor workflows programmatically. 
- It is designed to execute a series of tasks following specified dependencies on a specified schedule. 

### Installation and How to use Airflow

To use Airflow we run a web server and then access the UI through the browser. You can schedule jobs to run automatically, so besides the server you'll also need to run the scheduler. In a production setting we usually run it on a dedicated server, but here we'll run it locally to understand how it works. 
By default, your entire airflow directory is located at ~/airflow . But if you choose to change the default directory, simply runs this command:
export AIRFLOW_HOME="<YOUR_AIRFLOW_DIRECTORY>"
You'll create a virtual environment and run these commands to do install everything:
```bash
Create an environment for Airflow (Optional but recommended)
$ python3 -m venv .env
$ source .env/bin/activate
# Create a folder and set it as Airflow home
# Airflow needs a home. `~/airflow` is the default, but you can put it
# somewhere else if you prefer (optional)
mkdir -p ~/airflow/dags
export AIRFLOW_HOME='~/airflow'
export PATH=$PATH:~/.local/bin

# Install Airflow using the constraints file
AIRFLOW_VERSION=2.4.1
PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
# For example: 3.7
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
# For example: https://raw.githubusercontent.com/apache/airflow/constraints-2.4.1/constraints-3.7.txt
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"

Next set up the Airflow database and user.
# Init airflow. After init, you can adjust airflow settings in airflow.cfg
cd ~/airflow
airflow db init
# The Standalone command will initialise the database, make a user,
# and start all components for you.
airflow standalone

# Visit localhost:8080 in the browser and use the admin account details
# shown on the terminal to login.
# Enable the example_bash_operator dag in the home page
```

Note: If port 8080 occupied by other process and you want to free it to Airflow:

1) Find out the process ID (PID) which is occupying the port number (e.g., 8080) you would like to free
```bash
sudo lsof -i :8080
```
2) Kill the process which is currently using the port using its PID

```bash
sudo kill -9 PID
```
Before proceeding, we are required to create a username and password by running the following command (remember to change the firstname, lastname, and email fields to yours):
```bash
airflow users create \
    --username admin \
    --firstname <YOUR_FIRST_NAME> \
    --lastname <YOUR_LAST_NAME> \
    --role Admin \
    --email <YOUR_EMAIL>@<YOUR_EMAIL_HOST>
```

Apache Airflow consists of two core parts - Webserver and Scheduler. You'll have to run both to inspect and run your DAGs.
First, start the Webserver in the daemon mode (as a background process):
```
airflow webserver -D
```
Once it's running, use a similar command to run the Scheduler:
```
airflow scheduler -D
```
*** To find your DAG object list you can run __airflow dags list__ and confirm that your DAG shows up in the list.

#### How to Remove Airflow Example DAGs

You could delete the DAGs one by one, but there's a better approach. Open the ``` ~/airflow/airflow.cfg ``` file and change the load_examples value to False:

Save the file and reopen the terminal. You'll have to reset the Airflow database:
```bash
airflow db reset
```
Once done, start  the Airflow webserver and scheduler once again:
```bash
airflow webserver -D
airflow scheduler -D
```
#### What's Inside the Airflow SQLite Database?
Our Airflow instance is using SQLite for the database. It's not recommended for production environments, but will serve us fine for local development. Establish a connection to ~/airflow/airflow.db through some DBMS (I'm using TablePlus) and run the following SQL query:
```
SELECT * FROM sqlite_master
WHERE type = 'table';
```
#### Deployment Using Docker
- We can use Docker to deploy all of the components in Airflow architecture to a server effortlessly.
- Make sure Docker is installed. If it is not, you will have to install Docker and docker-compose for this to work. 
- This guide is using __docker-airflow managed by puckel__.
- Fork the below repo to your GitHub account and clone the forked repo to your machine.
```url
# Replace the URL with your forked repo
git clone https://github.com/puckel/docker-airflow
```
# Uncomment these lines to persist data on the local filesystem.                
  - PGDATA=/var/lib/postgresql/data/pgdata        
volumes:
  - ./pgdata:/var/lib/postgresql/data/pgdata
```
- Add the following codes to environment variables of webserver, worker, and scheduler to enable basic login. 

```bash
- AIRFLOW__WEBSERVER__AUTHENTICATE=True
- AIRFLOW__WEBSERVER__AUTH_BACKEND=airflow.contrib.auth.backends.password_auth
```

- Note that you have to create an admin user using the following python script or using Airflow CLI.
```python

from __future__ import print_function

import sys
from airflow import models, settings
from airflow.contrib.auth.backends.password_auth import PasswordUser

user = PasswordUser(models.User())
user.username = sys.argv[1]
user.email = sys.argv[2]
user.password = sys.argv[3]
session = settings.Session()
session.add(user)
try:
    session.commit()
except:
    print("User already exists.")
session.close()
exit()
```

#### Config Airflow:
-  In the repo, you will find a file named docker-compose-CeleryExecutor.yml.
-  This is the file that docker-compose config file for your Airflow deployment.
-  Uncomment the three lines like shown below, to persist the metadata database file to your system file
```text

### Workflows

- Workflows are defined as code, allowing them to be easily maintained, versioned, and tested. 
- a workflow is defined as a DAG (Directed Acyclic Graph), which contains individual units of work called Tasks. 
- Tasks have dependencies and branches. 

![Workflows](/Airflow-lesson/images/202111_ApacheAirflow_01.jpg?raw=true "Workflows")


### What is a DAG?

- DAG stands for Directed Acyclic Graph. 
- It is a graph with nodes, directed edges and no cycles. 
- A DAG is a data pipeline in Apache Airflow. 
- So, whenever you read “DAG”, it means “data pipeline”. 
- When a DAG is triggered, a DAGRun is created. A DAGRun is an instance of your DAG with an execution date in Airflow.
### Dependencies?
- DAG has directed edges. Those directed edges are the dependencies in an Airflow DAG between all of your operators/tasks. 
- If you want to say “Task A is executed before Task B”, you have to defined the corresponding dependency.
- The DAG itself doesn’t care about what is happening inside the tasks; it is merely concerned with how to execute them - the order to run them in, how many times to retry them, if they have timeouts, and so on.


```
task_a >> task_b
# Or
task_b << task_a
```

- The >> and << respectively mean “right bitshift” and “left bitshift” or “set downstream task” and “set upstream task”. 
- In the example, on the first line we say that task_b is a downstream task to task_a. 
- On the second line we say that task_a is an upstream task of task_b. Don’t worry, we will come back at dependencies.

![Workflows](/Airflow-lesson/images/airflow_dag.png?raw=true "DAG")

### What is Airflow Operator?

In an Airflow DAG, nodes are operators. In other words, a task in your DAG is an operator. An Operator is a class encapsulating the logic of what you want to achieve. For example, you want to execute a python function, you will use the PythonOperator. You want to execute a Bash command, you will use the BashOperator. Airflow brings a ton of operators that you can find here and here. When an operator is triggered, it becomes a task, and more specifically, a task instance. Below is example for Airflow Operator

![Operator](/Airflow-lesson/images/operators.png?raw=true "Operator Example")

### Declaring a DAG

**There are three ways to declare a DAG 
1) Context manager, which will add the DAG to anything inside it implicitly 
2) Standard constructor, passing the dag into any operators you use
3) use the ``` @dag ``` decorator to turn a function into a DAG generator

#### Using Context manager
```python
with DAG(
    "my_dag_name", start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
    schedule="@daily", catchup=False
) as dag:
    op = EmptyOperator(task_id="task")
```
#### Using Standard Constractor
```python
my_dag = DAG("my_dag_name", start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
             schedule="@daily", catchup=False)
op = EmptyOperator(task_id="task", dag=my_dag)
```
#### Using the @dag decorator
```
@dag(start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
     schedule="@daily", catchup=False)
def generate_dag():
    op = EmptyOperator(task_id="task")

dag = generate_dag()
```
- DAGs are nothing without Tasks to run, and those will usually either come in the form of either Operators, Sensors or TaskFlow.


### Example 1: First Airflow DAG


![AirflowDAG](/Airflow-lesson/images/AirflowDAG.png?raw=true "FirstDAG")

#### Step 1: Import DAG classes

- If you want to execute a Python function, you have to import the PythonOperator. 
- If you want to execute a bash command, you have to import the BashOperator. 
- Finally, the last import is usually the datetime class as you need to specify a start date to your DAG.

```python 3
from airflow import DAG
from airflow.operators.python import PythonOperator, BranchPythonOperator
from airflow.operators.bash import BashOperator
from datetime import datetime
```
#### Step 2: Create the Airflow DAG object
- A DAG object must have two parameters, a ``` dag_id ``` and a ``` start_date ```. 
- The __dag_id__ is the unique identifier of the DAG across all of DAGs. Each DAG must have a unique dag_id. 
- The __start_date__ defines the date at which your DAG starts being scheduled.
- If the start_date is set in the past, the scheduler will try to backfill all the non-triggered DAG Runs between the start_date and the current date. 
- Other arguments are the __schedule_interval__ and the __catchup__ arguments.
- The schedule_interval defines the interval of time at which your DAG gets triggered. 
  e.g Every 10 mins, every day, every month and so on. 
- 2 ways to define it, either with a CRON expression or with a timedelta object. The first option is the most often used.
- The catchup argument allows you to prevent from backfilling automatically the non triggered DAG Runs between the start date of your DAG and the current date. 
- If you want don’t want to end up with many DAG runs running at the same time, it’s usually a best practice to set it to False.
- You can use the with statment. "with” is a context manager and allows you to better manager objects. In that case, a DAG object.
```python 3
with DAG("my_dag", # Dag id
start_date=datetime(2021, 1 ,1), # start date, the 1st of January 2021 
schedule_interval='@daily',  # Cron expression, here it is a preset of Airflow, @daily means once every day.
catchup=False  # Catchup 
) as dag:

```

#### Step 3: Add tasks
- Training model A, B and C, are implemented with the PythonOperator. 

```python 3
from random import randint # Import to generate random numbers
def _training_model():
  return randint(1, 10) # return an integer between 1 - 10
with DAG(...) as dag:
# Tasks are implemented under the dag object
training_model_A = PythonOperator(
task_id="training_model_A",
python_callable=_training_model
)
training_model_B = PythonOperator(
task_id="training_model_B",
python_callable=_training_model
)
training_model_C = PythonOperator(
task_id="training_model_C",
python_callable=_training_model
)

```
or we can generet the above dynamically.

```python
    training_model_tasks = [
PythonOperator(
task_id=f"training_model_{model_id}",
python_callable=_training_model,
op_kwargs={
"model": model_id
}
) for model_id in ['A', 'B', 'C']
]
```
