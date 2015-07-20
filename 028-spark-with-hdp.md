# Apache Spark 1.3.1 with HDP 2.3

This Apache Spark 1.3.1 with HDP 2.3 guide lets you evaluate Apache Spark 1.3.1 on YARN with HDP 2.3.

Hortonworks recently announced general availability of Spark 1.3.1 on the HDP platform. Apache Spark is a fast moving community and Hortonworks plans frequent releases to allow evaluation and production use of the latest Spark technology on HDP for our customers.

With YARN, Hadoop can now support various types of workloads; Spark on YARN becomes yet another workload running against the same set of hardware resources.

This technical preview describes how to:

  * Run Spark on YARN and run the canonical Spark examples: SparkPi and Wordcount.
  * Run Spark 1.3.1 on HDP 2.3.
  * Use Spark DataFrame API
  * Work with a built-in UDF, collect_list, a key feature of Hive 13. This technical preview provides support for Hive 0.13.1 and instructions on how to call this UDF from Spark shell.
  * Use SparkSQL thrift JDBC/ODBC Server.
  * View history of finished jobs with Spark Job History.
  * Use ORC files with Spark, with examples.

When you are ready to go beyond these tasks, try the machine learning examples at Apache Spark.

### HDP Cluster Requirement

Spark 1.3.1 can be configured on any HDP 2.3 cluster whether it is a multi node cluster or a single node HDP Sandbox.

The instructions in this guide assumes you are using the latest Hortonworks Sandbox

### **Run the Spark Pi Example**

To test compute intensive tasks in Spark, the Pi example calculates pi by “throwing darts” at a circle. The example points in the unit square ((0,0) to (1,1)) and sees how many fall in the unit circle. The fraction should be pi/4, which is used to estimate Pi.

To calculate Pi with Spark:

  1. **Change to your Spark directory and become spark OS user:**

    cd /usr/hdp/current/spark-client
    su spark

  2. **Run the Spark Pi example in yarn-client mode:**

    ./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn-client --num-executors 3 --driver-memory 512m --executor-memory 512m --executor-cores 1 lib/spark-examples*.jar 10

**Note:** The Pi job should complete without any failure messages and produce output similar to below, note the value of Pi in the output message:
![](https://www.dropbox.com/s/i93qmcvjmzue1ho/Screenshot%202015-07-20%2014.48.48.png?dl=1)

### **Using WordCount with Spark**

#### **Copy input file for Spark WordCount Example**

Upload the input file you want to use in WordCount to HDFS. You can use any text file as input. In the following example, log4j.properties is used as an example:

As user spark:

    hadoop fs -copyFromLocal /etc/hadoop/conf/log4j.properties /tmp/data

### **Run Spark WordCount**

To run WordCount:

#### Run the Spark shell:

    ./bin/spark-shell --master yarn-client --driver-memory 512m --executor-memory 512m

Output similar to below displays before the Scala REPL prompt, scala>:
![](https://www.dropbox.com/s/f5bq5biq88gc2bs/Screenshot%202015-07-20%2014.50.31.png?dl=1)

#### At the Scala REPL prompt enter:**

```scala
val file = sc.textFile("/tmp/data")
val counts = file.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)
counts.saveAsTextFile("/tmp/wordcount")
```

##### Viewing the WordCount output with Scala Shell

To view the output in the scala shell:

  scala > counts.count()

![](https://www.dropbox.com/s/0j1yk2okljlqx3k/Screenshot%202015-07-20%2014.55.13.png?dl=1)

To print the full output of the WordCount job:

  scala > counts.toArray().foreach(println)

![](https://www.dropbox.com/s/942krdi729qbkzx/Screenshot%202015-07-20%2014.57.10.png?dl=1)

##### Viewing the WordCount output with HDFS

To read the output of WordCount using HDFS command:
Exit the scala shell.

  scala > exit

View WordCount Results:

  hadoop fs -ls /tmp/wordcount

It should display output similar to:

![](https://www.dropbox.com/s/tg17aqu3kieb9wa/Screenshot%202015-07-20%2014.58.22.png?dl=1)

Use the HDFS cat command to see the WordCount output. For example,

    hadoop fs -cat /tmp/wordcount/part-00000

![](https://www.dropbox.com/s/xed8ikl35jj8dx7/Screenshot%202015-07-20%2014.59.10.png?dl=1)

##### Using Spark DataFrame API

With Spark 1.3.1, DataFrame API is a new feature. DataFrame API provide easier access to data since it looks conceptually like a Table and a lot of developers from Python/R/Pandas are familiar with it.

Let's upload people text file to HDFS

    cd /usr/hdp/current/spark-client

    su spark
    hdfs dfs -copyFromLocal examples/src/main/resources/people.txt people.txt

    hdfs dfs -copyFromLocal examples/src/main/resources/people.json people.json

![](https://www.dropbox.com/s/g8igatmz9v7n4hy/Screenshot%202015-07-20%2015.01.49.png?dl=1)

Then let's launch the Spark Shell

    cd /usr/hdp/current/spark-client
    su spark
    ./bin/spark-shell --num-executors 2 --executor-memory 512m --master yarn-client

At the Spark Shell type the following:

    scala>val df = sqlContext.jsonFile("people.json")

This will produce and output such as

![](https://www.dropbox.com/s/7dk2nsnfrvkv4y4/Screenshot%202015-07-20%2015.04.33.png?dl=1)

**Note:** The highlighted output shows the inferred schema of the underlying people.json.

Now print the content of DataFrame with df.show

    scala>df.show

![](https://www.dropbox.com/s/keq8tjfkz4fjfjb/Screenshot%202015-07-20%2015.06.29.png?dl=1)

##### Data Frame API examples

    scala>import org.apache.spark.sql.functions._
    // Select everybody, but increment the age by 1
    scala>df.select(df("name"), df("age") + 1).show()

![](https://www.dropbox.com/s/n8lmf0lg4ed6t54/Screenshot%202015-07-20%2015.13.59.png?dl=1)

    // Select people older than 21
    scala>df.filter(df("age") > 21).show()

![](https://www.dropbox.com/s/2yjemit47m1eo3v/Screenshot%202015-07-20%2015.14.47.png?dl=1)

    // Count people by age
    df.groupBy("age").count().show()
![](https://www.dropbox.com/s/y5mk0zdxzi8si0t/Screenshot%202015-07-20%2015.15.41.png?dl=1)

##### Programmatically Specifying Schema

    import org.apache.spark.sql._
    val sqlContext = new org.apache.spark.sql.SQLContext(sc)
    val people = sc.textFile("people.txt")
    val schemaString = "name age"
    import org.apache.spark.sql.types.{StructType,StructField,StringType}
    val schema = StructType(schemaString.split(" ").map(fieldName => StructField(fieldName, StringType, true)))

![](https://www.dropbox.com/s/21m81un74ml65dd/Screenshot%202015-07-20%2015.18.02.png?dl=1)

```scala
val rowRDD = people.map(_.split(",")).map(p => Row(p(0), p(1).trim))
val peopleDataFrame = sqlContext.createDataFrame(rowRDD, schema)
peopleDataFrame.registerTempTable("people")
val results = sqlContext.sql("SELECT name FROM people")
results.map(t => "Name: " + t(0)).collect().foreach(println)
```
This will produce an output like

![](https://www.dropbox.com/s/t3x20c5fs2dh2o1/Screenshot%202015-07-20%2015.19.49.png?dl=1)

### Running Hive 0.13.1 UDF

Hive 0.13.1 provides a new built-in UDF collect_list(col) which returns a list of objects with duplicates.
The below example reads and write to HDFS under Hive directories. In a production environment one needs appropriate HDFS permission. However for evaluation you can run all this section as hdfs user.

Before running Hive examples run the following steps:

#### Launch Spark Shell on YARN cluster

    su hdfs
    ./bin/spark-shell --num-executors 2 --executor-memory 512m --master yarn-client

#### Create Hive Context

    scala> val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)

You should see output similar to the following:

![](https://www.dropbox.com/s/aga459qghah9x15/Screenshot%202015-07-20%2015.24.12.png?dl=1)

#### Create Hive Table

    scala> hiveContext.sql("CREATE TABLE IF NOT EXISTS TestTable (key INT, value STRING)")

You should see output similar to the following:

![](https://www.dropbox.com/s/h23w2qf99arqfjk/Screenshot%202015-07-20%2015.25.34.png?dl=1)

#### Load example KV value data into Table

    scala> hiveContext.sql("LOAD DATA LOCAL INPATH 'examples/src/main/resources/kv1.txt' INTO TABLE TestTable")

You should see output similar to the following:

![](https://www.dropbox.com/s/pqu1u95ep91omna/Screenshot%202015-07-20%2015.26.53.png?dl=1)

#### **Invoke Hive collect_list UDF**

    scala> hiveContext.sql("from TestTable SELECT key, collect_list(value) group by key order by key").collect.foreach(println)

You should see output similar to the following:

    …
    [489,ArrayBuffer(val_489, val_489, val_489, val_489)]
    [490,ArrayBuffer(val_490)]
    [491,ArrayBuffer(val_491)]
    [492,ArrayBuffer(val_492, val_492)]
    [493,ArrayBuffer(val_493)]
    [494,ArrayBuffer(val_494)]
    [495,ArrayBuffer(val_495)]
    [496,ArrayBuffer(val_496)]
    [497,ArrayBuffer(val_497)]
    [498,ArrayBuffer(val_498, val_498, val_498)]

### **Read & Write ORC File Example**

In this tech preview, we have implemented full support for ORC files with Spark. We will walk through an example that reads and write ORC file and uses ORC structure to infer a table.

### **ORC File Support**

#### **Create a new Hive Table with ORC format**

    scala>hiveContext.sql("create table orc_table(key INT, value STRING) stored as orc")

#### **Load Data into the ORC table**

    scala>hiveContext.sql("INSERT INTO table orc_table select * from testtable")

#### **Verify that Data is loaded into the ORC table**

    scala>hiveContext.sql("FROM orc_table SELECT *").collect().foreach(println)

#### **Read ORC Table from HDFS as HadoopRDD**

    scala> val inputRead = sc.hadoopFile("/apps/hive/warehouse/orc_table", classOf[org.apache.hadoop.hive.ql.io.orc.OrcInputFormat],classOf[org.apache.hadoop.io.NullWritable],classOf[org.apache.hadoop.hive.ql.io.orc.OrcStruct])

#### **Verify we can manipulate the ORC record through RDD**

    scala> val k = inputRead.map(pair => pair._2.toString)
    scala> val c = k.collect

You should see output similar to the following:

    ...
    14/12/22 18:41:37 INFO scheduler.DAGScheduler: Stage 7 (collect at :16) finished in 0.418 s
    14/12/22 18:41:37 INFO scheduler.DAGScheduler: Job 4 finished: collect at :16, took 0.437672 s
    c: Array[String] = Array({238, val_238}, {86, val_86}, {311, val_311}, {27, val_27}, {165, val_165}, {409, val_409}, {255, val_255}, {278, val_278}, {98, val_98}, {484, val_484}, {265, val_265}, {193, val_193}, {401, val_401}, {150, val_150}, {273, val_273}, {224, val_224}, {369, val_369}, {66, val_66}, {128, val_128}, {213, val_213}, {146, val_146}, {406, val_406}, {429, val_429}, {374, val_374}, {152, val_152}, {469, val_469}, {145, val_145}, {495, val_495}, {37, val_37}, {327, val_327}, {281, val_281}, {277, val_277}, {209, val_209}, {15, val_15}, {82, val_82}, {403, val_403}, {166, val_166}, {417, val_417}, {430, val_430}, {252, val_252}, {292, val_292}, {219, val_219}, {287, val_287}, {153, val_153}, {193, val_193}, {338, val_338}, {446, val_446}, {459, val_459}, {394, val_394}, {2…

#### **Copy example table into HDFS**

    su hdfs
    cd SPARK_HOME
    hadoop dfs -put examples/src/main/resources/people.txt people.txt

#### **Run Spark-Shell**

    ./bin/spark-shell --num-executors 2 --executor-memory 512m --master yarn-client

on Scala prompt type the following, except for the comments

    import org.apache.spark.sql.hive.orc._
    import org.apache.spark.sql._
    # Load and register the spark table
    val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)
    val people = sc.textFile("people.txt")
    val schemaString = "name age"
    val schema = StructType(schemaString.split(" ").map(fieldName => {if(fieldName == "name") StructField(fieldName, StringType, true) else StructField(fieldName, IntegerType, true)}))
    val rowRDD = people.map(_.split(",")).map(p => Row(p(0), new Integer(p(1).trim)))
    # Infer table schema from RDD
    val peopleSchemaRDD = hiveContext.applySchema(rowRDD, schema)
    # Create a table from schema
    peopleSchemaRDD.registerTempTable("people")
    val results = hiveContext.sql("SELECT * FROM people")
    results.map(t => "Name: " + t.toString).collect().foreach(println)
    # Save Table to ORCFile
    peopleSchemaRDD.saveAsOrcFile("people.orc")
    # Create Table from ORCFile
    val morePeople = hiveContext.orcFile("people.orc")
    morePeople.registerTempTable("morePeople")
    hiveContext.sql("SELECT * from morePeople").collect.foreach(println)

### **SparkSQL Thrift Server for JDBC/ODBC access**

With this Tech Preview SparkSQL’s thrift server provides JDBC access to SparkSQL.

  1. **Start Thrift Server**
From SPARK_HOME, start SparkSQL thrift server, Note the port value of the thrift JDBC server

    su spark
    ./sbin/start-thriftserver.sh --master yarn-client --executor-memory 512m --hiveconf hive.server2.thrift.port=10001

  * **Connect to Thrift Server over beeline**
Launch beeline from SPARK_HOME

    su spark
    ./bin/beeline

  * **Connect to Thrift Server & Issue SQL commands**
On beeline prompt

    beeline>!connect jdbc:hive2://localhost:10001

Note this is example is without security enabled, so any username password should work.

Note, the connection may take a few second to be available and try show tables after a wait of 10-15 second in a Sandbox env.

    0: jdbc:hive2://localhost:10001> show tables;
    +------------+--------------+
    | tableName | isTemporary |
    +------------+--------------+
    | orc_table | false |
    | testtable | false |
    +------------+--------------+
    2 rows selected (1.275 seconds)
    0: jdbc:hive2://localhost:10001> exit

  * **Stop Thrift Server**

    ./sbin/stop-thriftserver.sh

### **Spark Job History Server**

Spark Job history server is integrated with YARN’s Application Timeline Server(ATS) and publishes job metrics to ATS. This allows job details to be available after the job finishes. Y

  1. **Start Spark History Server**

    ./sbin/start-history-server.sh

You can let the history server run, while you run examples in the tech preview and go to YARN resource manager page at http://:8088/cluster/apps and see the logs of finished application with the history server.

  2. **Stop Spark History Server**

    ./sbin/stop-history-server.sh
