# SparkSlurm
Notes and scripts for running Spark on a shared slurm cluster.

I am running spark in the standalone mode: [docs](http://spark.apache.org/docs/latest/spark-standalone.html).



## Setup
Steps:
* Download a pre-built version of spark that contains hadoop [downloads](http://spark.apache.org/downloads.html).
* Extract it to a shared location on the cluster.
* Set the SPARK_HOME environment variable. eg. `export SPARK_HOME=.../spark-1.6.1-bin-hadoop2.6`.

I was able to run it successfuly with `spark-1.6.1-bin-hadoop2.6`.
I had issues with `spark-1.6.1-bin-without-hadoop.tgz` failing when running under linux.


## Modes of Operation
There are three ways you can run spark on a shared slurm cluster: an interactive session utilizing a single node, a sbatch run utilizing a single node and a sbatch run using multiple nodes. One more possibility not presented here is to setup a permanent cluster.

__Single Node interactive session__

From the cluster login node.

Run srun in interactive mode with the number of CPU, memory and optionally a max time you are going to use it for.
So for example to limit it 10 minutes, use 8 CPUs and 4G of memory per CPU/executor on the 'interactive' partition:

```
srun -t 00:10:00 -c 8 --pty --mem-per-cpu 4000 -p interactive bash -i
```

If necessary load modules for newer java and python.
On our cluster this is what I currently use:
```
module load jdk/1.8.0_45-fasrc01
module load python/2.7.9-fasrc01
```
Run the spark provided PI calculation specifying the same settings as above.

```
$SPARK_HOME/bin/spark-submit \
       --driver-memory 4G \
       --total-executor-cores 8 \
       --executor-memory 4G \
       $SPARK_HOME/examples/src/main/python/pi.py 10
```  
This should print out a good bit of progress log information.
If it worked towards the end of the output you should see something like this:
```
Pi is roughly 3.155600
```


