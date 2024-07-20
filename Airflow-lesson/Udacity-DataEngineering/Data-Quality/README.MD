## Data Quality

Topics covered

* What is data lineage and why it's critical to defining and maintaining your data pipelines?
* Data pipeline schedules and learn how to leverage them to make our pipelines more manageable.
* Data partitioning and see how partitioning can make our pipelines faster and more reliable.
* Overview of data quality, how it's defined, 
* and how we can build and test our data pipelines to ensure that our data pipelines are working as intended.

#### Data lineage
- The data lineage of a dataset describes the discrete steps involved in the creation, movement, and calculation of that dataset.
- It includes the data origin, what happens to it, and where it moves over time.
- Data lineage gives visibility while greatly simplifying the ability to trace errors back to the root cause in a data analytics process.
- It also enables replaying specific portions or inputs of the data flow for step-wise debugging or regenerating lost output.

#### Why is Data Lineage important?
__Instilling Confidence:__ Being able to describe the data lineage of a particular dataset or analysis will build confidence in data consumers (engineers, analysts, data scientists, etc.) that our data pipeline is creating meaningful results using the correct datasets. If the data lineage is unclear, its less likely that the data consumers will trust or use the data.
__Defining Metrics:__ Another major benefit of surfacing data lineage is that it allows everyone in the organization to agree on the definition of how a particular metric is calculated.
__Debugging:__ Data lineage helps data engineers track down the root of errors when they occur. If each step of the data movement and transformation process is well described, it's easy to find problems when they occur.
