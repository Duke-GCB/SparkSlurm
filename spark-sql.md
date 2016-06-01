## Spark SQL

Spark provides a SQL repl named `spark-sql` similar to `sqlite3` or `psql`.
Spark's SQL implementation doesn't implement all of the standard but it does support a good bit.
Using SQL you can define _tables_ that read and write to different types of files(csv, tsv, json, parquet).

Parquet is the native file format for Spark. It contains not only the data but metadata about columns involved. 
Spark SQL has special syntax to make reading parquet files easy. Performance is supposed to be improved for 
reading this file format.

_Something to keep in mind is making sure slurm and spark are both aware of what resources we are using._

Spark has some documentation on using SQL but it is a little sparse: [Spark SQL, DataFrames and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html)

## Getting Started
Download spark and set $SPARK_HOME, $SPARK_BIN and $SPARK_SBIN as outlined in the __Setup__ step from these [Instructions](https://github.com/Duke-GCB/SparkSlurm/blob/master/README.md#setup).

## Run on an interactive node
You need to decide how many CPU and how much memory to use for your workload.
The rest of the steps will assume 8 CPU and 4G memory.

#### Launch an interactive spark job
```
srun -t 00:20:00 -c 8 --pty --mem-per-cpu 4000 -p interactive,all,new bash -i
```
This sets 20 min timeout, enabling interactive terminal and gives a list of partitions to try and find resources for our node "interactive,all,new". It should return you to a prompt on the node we will use for spark-sql.

#### Start spark-sql repl
```
$SPARK_BIN/spark-sql --packages com.databricks:spark-csv_2.10:1.4.0 \ 
  --total-executor-cores 7 --executor-memory 4G --driver-memory 4G 
```
By default different spark commands try to take up all available memory and CPU so it is important to specify these items.
We are using 1 core for the 'driver' and 87 cores for the 'executors'.

After a few lines of output you should see the spark-sql prompt:
```
spark-sql>
```


#### SQL commands
* `source <sql_filename>;` - executes the SQL in _sql_filename_
* `show tables;` - prints out a list of table names
* `describe <tablename>;` - prints out columns and their types for _tablename_
* `quit;` - exit the spark REPL

Keep in mind all commands must end with a ';'.
