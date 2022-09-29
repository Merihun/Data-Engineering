# Apache AirFlow

- Airflow is a platform created by the community to programmatically author, schedule and monitor workflows.
- AirFlow is a Workflow Scheduling and Monitoring Platform
- Airflow was started by Airbnb in 2014. In 2016 it became an Apache incubator and in 2019 it was adopted as an Apache software foundation project.
- It is a platform written in Python to schedule and monitor workflows programmatically. 
- It is designed to execute a series of tasks following specified dependencies on a specified schedule. 
### Workflows

- Workflows are defined as code, allowing them to be easily maintained, versioned, and tested. 
- a workflow is defined as a DAG (Directed Acyclic Graph), which contains individual units of work called Tasks. Tasks have dependencies and branches. 
https://www.indellient.com/wp-content/uploads/2021/11/202111_ApacheAirflow_01.jpg![image](https://user-images.githubusercontent.com/26862785/193139227-e5d42981-6494-4ced-9b4e-9160c17ec7cd.png)


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
