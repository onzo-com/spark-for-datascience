# Spark for data science

## What is Spark
Spark is a tool for working with large datasets. A Spark cluster allows a client to load and analyse a large amount of data. By distributing the data around a cluster of machines it is able to effectively scale operations on very large datasets, whilst maintaining a simple abstraction for a data scientist or developer to work with.

## What is this repo
This repo aims to help a data scientist quickly get up to speed on using Spark for their research and development work. The intention is this will help people quickly be able to play with Spark, see if it meets their needs. If it does having a data science and engineering team working with the same tools can greatly increase productivity and the speed with which we can promote new algorithms to production.

## Contents
- [Setting up Spark and useful tools](#setup)
- [Setting up a local Spark instance](#setting-up-a-local-spark-instance)
- [Setting up your Jupyter notebook to work with spark](#using-pyspark-with-jupyter-notebook)

## Setup

### Setting up a local Spark instance
We recommend always setting up a virtual environment for your python project, the following instructions include creating a new directory for your work, but this can be omitted if you like!
```
export PROJECT_NAME=pyspark-playground
mkdir ${PROJECT_NAME}
cd ${PROJECT_NAME}
virtualenv .
. ./bin/activate
pip install pyspark
```

You can now run `pyspark` and run Spark commands! Try it out with the following snippet!
```
import random
num_samples = 100000000
def inside(p):     
  x, y = random.random(), random.random()
  return x*x + y*y < 1
count = sc.parallelize(range(0, num_samples)).filter(inside).count()
pi = 4 * count / num_samples
print(pi)
sc.stop()
```

### Using pyspark with Jupyter notebook
There are 2 approaches to using Jupyter notebooks:

1) [Jupyter notebook only - local Spark cluster](#jupyter-notebook-only-local-spark-cluster) - 
Setting up so pyspark will always run Jupyter notebook and you'll have access to Spark functions within it - suitable 
if you will always use Jupyter notebooks for running your code, and don't need the power of a remote spark cluster
1) [Jupyter notebook or IDE of choice - local Spark cluster](#jupyter-notebook-or-ide-of-choice-local-spark-cluster) - 
Setting up to find your local cluster - useful if you don't need the power of a remote spark cluster. You can use this 
with Jupyter notebooks or with your IDE of choice
1) [Jupyter notebook or IDE of choice - remote Spark cluster](#jupyter-notebook-or-ide-of-choice-remote-spark-cluster) - 
Setting up to connect to a remote cluster - useful if you need the power of a remote cluster

#### Jupyter notebook only local Spark cluster
Add the following exports to your ~/.bashrc file:
```
# Use jupyter notebooks to run spark
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook'
```
Now when you run pyspark it will open up a jupyter notebook with access to a spark context (core part of the spark API).

#### Jupyter notebook or IDE of choice local Spark cluster
In any Jupyter notebook or IDE you can very easily setup to give access to a local Spark context too. First install
findspark and open up a Jupyter notebook (or your IDE of choice)
```
pip install findspark
jupyter notebook
```
Within the notebook (or IDE) add the following code to the top of your file:
```
import findspark
findspark.init()
import pyspark
import random
sc = pyspark.SparkContext(appName="myAppNameGoesHere")
```
You'll now be able to run Spark commands, simples!

#### Jupyter notebook or IDE of choice remote Spark cluster
**WARNING - this section hasn't been tested with a remote cluster yet!!**
Similar to the last snippet, except explicitly setting the Spark cluster to connect to. Just add the following
to your Jupyter notebook or code in your IDE:
```
import findspark
findspark.init()
import pyspark
conf = pyspark.SparkConf().setMaster("local").setAppName("My App") # Replace local with your remote Spark cluster address
sc = pyspark.SparkContext(conf = conf)
```
