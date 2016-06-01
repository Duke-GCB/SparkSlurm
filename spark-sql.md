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
