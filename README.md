# Spark Usage in Windows
This document serves as a reference for how to accomplish different tasks in spark on windows. It includes both local and cluster methods.

## Spark Configuration
--driver-memory to max --executor-memory for cluster

## Setting up Python Notebooks
Pyspark can be manipulated with environmental variables to direct python to open in a number of ways. You can use the Ipython shell or a notebook such as Jupyter.

Setup Jupyter notebook:
```
set PYSPARK_DRIVER_PYTHON=Jupyter
set PYSPARK_DRIVER_PYTHON_OPTS='notebook'
```
Setup Ipython shell:
```
set PYSPARK_DRIVER_PYTHON=Ipython
```
You can also setup an Ipython notebook by changing the OPTS value to notebook, but Jupyter is the favored option for this use.

Just as with opening the Jupyter or Ipython from the command line there are numerous options that can be set with --*option* in the `PYSPARK_DRIVER_PYTHON_OPTS` variable. Reference the docs for [Ipython](https://ipython.org/ipython-doc/3/config/intro.html#setting-config), or [Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/config.html).

After setting these variables calling `pyspark` from the command line will launch in the method you defined.

## Switching Python Versions
If you have multiple versions of Python installed, you can switch the versions with the following:
```
set PYSPARK_PYTHON=*path/to/python
```

## Connecting to Mongodb
Spark can be used to connect to Mongodb for both read and write operations. This can be especially useful for non-indexed query operations and the query gets distributed on partitioned segments of the database.

When launching pyspark for use in a shell include the following option:
```
pyspark --pacakges org.mongodb.spark:mongo-spark-connector_2.11:2.0.0
```
**n.b. Be sure to verify your version of Scala and Spark. The end of the above statement should be %SCALA_VERSION%:%SPARK_MAJOR_VERSION%**

## Creating a Cluster
To create a cluster you must first register one machine as the master and subsequently add machines as slaves. Spark only contains bash scripts for stargin these so it is ideal to write your own. For detailed directions on setting up a cluster see [Spark Standalone Mode](http://spark.apache.org/docs/latest/spark-standalone.html).

Start Master:
```
spark-class org.apache.spark.deploy.master.Master
```
Start Worker:
```
spark-class org.apache.spark.deploy.worker.Worker spark:\\hostname:7077
```
**n.b. This must be done on each worker machine. Thus the importance of writing a script to automate startup.**

## Submitting Jobs to a Cluster
Once your cluster is setup you can submit jobs with the `spark-submit` script. Reference the [Submitting Applications](http://spark.apache.org/docs/latest/submitting-applications.html) page for more detailed information.  

```
spark-submit --master spark:\\hostname:7077 <options>
```
