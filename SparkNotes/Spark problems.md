## Spark problems

#### How to redirect entire output of spark-submit to a file?
refer to [this SO problem](https://stackoverflow.com/questions/46429962/how-to-redirect-entire-output-of-spark-submit-to-a-file)
+ no stdout output
	+ ```spark-submit something.py > results.txt 2>&1```
	+ ```spark-submit sth.py &> results.txt```
+ redirect the stderr to stdout and then pipe it with ```tee```
	+ ```spark-submit sth.py > results.txt 2>&1 | tee -a results.txt```

#### DataFrame and DataSet
+ in Spark 3.x(or since Spark 2.0.0), DataFrame is just a type alias for Dataset[Row] in org.apache.spark.sql package object
	```
	package org.apache.spark
	package object sql {
		type DataFrame = Dataset[Row]
	}
	```

#### spark-submit results in out-of-memory GC
+ problem: 
+ resources:
	+ http://spark.apache.org/docs/latest/configuration.html#environment-variables
	+ http://spark.apache.org/docs/latest/spark-standalone.html#Executors%20Scheduling
	+ https://www.shuzhiduo.com/A/1O5Ej284d7/
+ decrease executor-core numbers
	```
	spark-submit --executor-cores 1
	```
+ increase driver memory
	```
	spark-submit --driver-memory 4G
	```

#### spark conf file indicates many of its default configurations
+ spark saves its output log defaultly in SPARK_HOME/logs
+ I am not able to understand the files in SPARK_HOME/logs
	+ I see there are 5 files, 2 of which are labelled with master, the others are labelled with worker
	+ master log files both ends with some string like "I have been elected leader ..."
	+ I have been running spark-submit tasks these days; but the log files still  belong to April 13th.

#### DataFrameWriter SaveMode
+ there are 4 SaveModes:
	```
	package org.apache.spark.sql;
	public enum SaveMode {
		  /**
		   * Append mode means that when saving a DataFrame to a data source, if data/table already exists,
		   * contents of the DataFrame are expected to be appended to existing data.
		   *
		   * @since 1.3.0
		   */
		  Append,
		  /**
		   * Overwrite mode means that when saving a DataFrame to a data source,
		   * if data/table already exists, existing data is expected to be overwritten by the contents of
		   * the DataFrame.
		   *
		   * @since 1.3.0
		   */
		  Overwrite,
		  /**
		   * ErrorIfExists mode means that when saving a DataFrame to a data source, if data already exists,
		   * an exception is expected to be thrown.
		   *
		   * @since 1.3.0
		   */
		  ErrorIfExists,
		  /**
		   * Ignore mode means that when saving a DataFrame to a data source, if data already exists,
		   * the save operation is expected to not save the contents of the DataFrame and to not
		   * change the existing data.
		   *
		   * @since 1.3.0
		   */
		  Ignore
		}
	```
+ when setting a dataframe's savemode, there are 2 ways:
	+ default savemode is ErrorIfExists
	```
	 /**
	   * Specifies the behavior when data or table already exists. Options include:
	   * <ul>
	   * <li>`SaveMode.Overwrite`: overwrite the existing data.</li>
	   * <li>`SaveMode.Append`: append the data.</li>
	   * <li>`SaveMode.Ignore`: ignore the operation (i.e. no-op).</li>
	   * <li>`SaveMode.ErrorIfExists`: throw an exception at runtime.</li>
	   * </ul>
	   * <p>
	   * The default option is `ErrorIfExists`.
	   *
	   * @since 1.4.0
	   */
	def mode(saveMode: SaveMode): DataFrameWriter[T] = {
    	this.mode = saveMode
    	this
  	}
	```
	```
	def mode(saveMode: String): DataFrameWriter[T] = {
	    saveMode.toLowerCase(Locale.ROOT) match {
	      case "overwrite" => mode(SaveMode.Overwrite)
	      case "append" => mode(SaveMode.Append)
	      case "ignore" => mode(SaveMode.Ignore)
	      case "error" | "errorifexists" | "default" => mode(SaveMode.ErrorIfExists)
	      case _ => throw new IllegalArgumentException(s"Unknown save mode: $saveMode. Accepted " +
	        "save modes are 'overwrite', 'append', 'ignore', 'error', 'errorifexists', 'default'.")
	    }
	  }
	```

+ `kubectl get pod -n spark-operator`

+ `kubectl get sparkapp`



