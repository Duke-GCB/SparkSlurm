# SparkSlurm
Running Spark on a Slurm cluster.
This works via spark in the standalone mode: [docs](http://spark.apache.org/docs/latest/spark-standalone.html).

## Setup
Steps:
* Download a version 2+ of spark that contains hadoop from [downloads](http://spark.apache.org/downloads.html).
* Extract it to a shared location on the cluster. Your home directory for example.
* Set the SPARK_HOME environment variable. eg. `export SPARK_HOME=.../spark-2.3.0-bin-hadoop2.7`.

## Start spark cluster
```
#!/bin/bash

# Make 3 nodes: 1 master 2 workers using 8 CPUs on each
# Allocate 4000 MB memory per CPU
#SBATCH -N 3
#SBATCH --mem-per-cpu 4000
#SBATCH --ntasks-per-node 1
#SBATCH --cpus-per-task 8

BASE_DIR=~
NUM_WORKERS=2
CORES=8
MEMORY=4G

export SPARK_HOME=$BASE_DIR/spark-2.3.0-bin-hadoop2.7
export SPARK_NO_DAEMONIZE=Y
MASTER=$(hostname)
export MASTER_SPARK_ADDR="spark://$MASTER:7077"
echo "Spark cluster master address: $MASTER_SPARK_ADDR

# load java and python modules
module load jdk/1.8.0_45-fasrc01
module load python/2.7.9-fasrc01

echo "starting master"
$SPARK_HOME/sbin/start-master.sh &
sleep 1

for i in $(seq $NUM_WORKERS)
do
   echo "starting slave $i"
   $SPARK_HOME/sbin/start-slave.sh $MASTER_SPARK_ADDR --cores $CORES --memory $MEMORY &
   sleep 1
done

echo "NOTE: You must scancel this process. It will continue to run."
wait
```

## Troubleshooting

If you have kryoserializer errors where file chunks where too big to be passed around. 
To fix this run:
```
cp $SPARK_HOME/conf/spark-defaults.conf.template $SPARK_HOME/conf/defaults.conf
```
Then add the following to the end of $SPARK_HOME/conf/defaults.conf
```
spark.kryoserializer.buffer.max    1g
```



