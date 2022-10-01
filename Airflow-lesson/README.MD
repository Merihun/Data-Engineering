# Apache AirFlow

- Airflow is a platform created by the community to programmatically author, schedule and monitor workflows.
- AirFlow is a Workflow Scheduling and Monitoring Platform
- Airflow was started by Airbnb in 2014. In 2016 it became an Apache incubator and in 2019 it was adopted as an Apache software foundation project.
- It is a platform written in Python to schedule and monitor workflows programmatically. 
- It is designed to execute a series of tasks following specified dependencies on a specified schedule. 

### Workflows

- Workflows are defined as code, allowing them to be easily maintained, versioned, and tested. 
- a workflow is defined as a DAG (Directed Acyclic Graph), which contains individual units of work called Tasks. Tasks have dependencies and branches. 

![Workflows](/Airflow-lesson/images/202111_ApacheAirflow_01.jpg?raw=true "Workflows")


### What is a DAG?

- DAG stands for Directed Acyclic Graph. 
- It is a graph with nodes, directed edges and no cycles. 
- A DAG is a data pipeline in Apache Airflow. 
- So, whenever you read “DAG”, it means “data pipeline”. 
- When a DAG is triggered, a DAGRun is created. A DAGRun is an instance of your DAG with an execution date in Airflow.

![Workflows](/Airflow-lesson/images/airflow_dag.png?raw=true "DAG")

### What is Airflow Operator?

In an Airflow DAG, nodes are operators. In other words, a task in your DAG is an operator. An Operator is a class encapsulating the logic of what you want to achieve. For example, you want to execute a python function, you will use the PythonOperator. You want to execute a Bash command, you will use the BashOperator. Airflow brings a ton of operators that you can find here and here. When an operator is triggered, it becomes a task, and more specifically, a task instance. Below is example for Airflow Operator

![Operator](/Airflow-lesson/images/airflow_dag.png?operators.png?raw=true "Operator Example")


#### Installation

- Airflow uses constraint files to enable reproducible installation, so using pip and constraint files is recommended.
```bash
# Airflow needs a home. `~/airflow` is the default, but you can put it
# somewhere else if you prefer (optional)
export AIRFLOW_HOME=~/airflow

# Install Airflow using the constraints file
AIRFLOW_VERSION=2.4.0
PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
# For example: 3.7
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
# For example: https://raw.githubusercontent.com/apache/airflow/constraints-2.4.0/constraints-3.7.txt
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"

# The Standalone command will initialise the database, make a user,
# and start all components for you.
airflow standalone

# Visit localhost:8080 in the browser and use the admin account details
# shown on the terminal to login.
# Enable the example_bash_operator dag in the home page

```
