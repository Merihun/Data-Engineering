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
### Dependencies?
- DAG has directed edges. Those directed edges are the dependencies in an Airflow DAG between all of your operators/tasks. 
- If you want to say “Task A is executed before Task B”, you have to defined the corresponding dependency.
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
