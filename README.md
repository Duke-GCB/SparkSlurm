# SparkSlurm
Running Spark on a Slurm cluster.
This works via spark in the standalone mode: [docs](http://spark.apache.org/docs/latest/spark-standalone.html).

## Setup
Steps:
* Download a version 2+ of spark that contains hadoop from [downloads](http://spark.apache.org/downloads.html).
* Extract it to a shared location on the cluster. Your home directory for example.
* Set the SPARK_HOME environment variable. eg. `export SPARK_HOME=.../spark-2.3.0-bin-hadoop2.7`.
* Download [spark.sbatch](https://github.com/Duke-GCB/SparkSlurm/blob/master/spark.sbatch) from this repo. Update `spark-2.3.0-bin-hadoop2.7` for the version of spark you downloaded. You may also edit this to increase the number of nodes/cpus/memory used by your spark cluster.

## Start spark cluster
Start a sbatch job that will run your cluster. 
This will start up multiple nodes and continue running until you `scancel` this job.
```
sbatch spark.sbatch
```
Check the slurm-*.out file created by this job. 
The top line should contain the spark master address. eg. `spark://<nodename>:7077`
This needs to be passed in to your spark commands.

## Run an example command against the spark cluster
```
SPARK_MASTER=spark://<nodename>:7077
$SPARK_HOME/bin/spark-submit --master $SPARK_MASTER $SPARK_HOME/examples/src/main/python/pi.py
```

## Stop spark cluster
```
scancel <JOBID>
```

## Run a spark command that will spin up a single node spark cluster
```
$SPARK_HOME/bin/spark-submit $SPARK_HOME/examples/src/main/python/pi.py
```

# Troubleshooting

If you have kryoserializer errors where file chunks where too big to be passed around. 
To fix this run:
```
cp $SPARK_HOME/conf/spark-defaults.conf.template $SPARK_HOME/conf/defaults.conf
```
Then add the following to the end of $SPARK_HOME/conf/defaults.conf
```
spark.kryoserializer.buffer.max    1g
```
