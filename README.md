# Cassandra-with-Springboot
Cassandra-with-Springboot -notes 


### Column family:
A column family is a database object that contains columns of related data. It is a tuple (pair) that consists of a key-value pair, where the key is mapped to a value that is a set of columns. In analogy with relational databases, a column family is as a "table", each key-value pair being a "row". Each column is a tuple (triplet) consisting of a column name, a value, and a timestamp. In a relational database table, this data would be grouped together within a table with other non-related data.


- A column family consists of multiple rows.
- Each row can contain a different number of columns to the other rows. And the columns don’t have to match the columns in the other rows (i.e. they can have different column names, data types, etc).
- Each column is contained to its row. It doesn’t span all rows like in a relational database. Each column contains a name/value pair, along with a timestamp. Note that this example uses Unix/Epoch time for the timestamp.

![Alt Text](https://i.stack.imgur.com/rPdYd.jpg )


          Book {

           key: 8812244567{ name: “Hadoop The Definitive Guide”, author:” Tom White”, publisher:”Oreilly”, priceInr;650, category: “hadoop”, edition:4},

           key: 8812244568{ name”” Hadoop in Action”, author: “Chuck Lam”, publisher:”manning”, priceInr;590, category: “hadoop”},

           key: 8812244569{ name:” Cassandra: The Definitive Guide”, author: “Eben Hewitt”, publisher:” Oreilly”, priceInr:600, category: “cassandra”},

          }


### Row store vs Column Store

Most traditional databases store data in the form of records. Each record contains a set of fields containing values that are related to each other. In a relational database, records are rows and fields are columns. In general, what we get is something like this table:

![Alt Text](https://blog.pythian.com/wp-content/uploads/RowStore-2018-03-16.png )

![Alt Text](https://phoenixnap.com/kb/wp-content/uploads/2020/05/column-oriented-database.png )

![Alt Text](https://dv-website.s3.amazonaws.com/uploads/2018/09/wcd-pic1.png )

What a column store does is defined in its name. Instead of storing data in rows or records the data is stored in columns.
when you want to compute the average age you only have to read the data with age information into memory instead of all the data in each row. 

![Alt Text](https://blog.pythian.com/wp-content/uploads/columnStore-2018-03-18.png)

- https://database.guide/what-is-a-column-store-database/

If you are going to store data in columns rather than rows the cost of doing single inserts is very high and the number of raw bytes is much larger than what you would normally store.

### Performance COlumn vs row database :

![Alt Text](https://blog.acolyer.org/wp-content/uploads/2018/09/columnstores-fig-1-2.jpeg?w=480)


### Cassandra :

In Cassandra this matters because the data in a particular column family is stored in the same files on disk - so it is more efficient to place data items that are likely to be retrieved together, in the same ColumnFamily. This is partly a practical speed concern, but also a matter of organising your data into a clear schema. This touches upon your second definition - one might consider all the data about a particular key to be a "row", but partitioned by Column Family. However, in Cassandra it is not really a single row, because the data in one ColumnFamily can be changed independently of the data in other ColumnFamilies for the same row key.

### Components of Cassandra

The key components of Cassandra are as follows −

### Node 

− It is the place where data is stored.

### Data center

− It is a collection of related nodes.

### Cluster 

− A cluster is a component that contains one or more data centers.

### Commit log 

− The commit log is a crash-recovery mechanism in Cassandra. Every write operation is written to the commit log.

### Mem-table

− A mem-table is a memory-resident data structure. After commit log, the data will be written to the mem-table. Sometimes, for a single-column family, there will be multiple mem-tables.

### SSTable

− It is a disk file to which the data is flushed from the mem-table when its contents reach a threshold value.

### Bloom filter

− These are nothing but quick, nondeterministic, algorithms for testing whether an element is a member of a set. It is a special kind of cache. Bloom filters are accessed after every query.



### Keyspace :
A keyspace in Cassandra is a namespace that defines data replication on nodes. A cluster contains one keyspace per node.The CREATE KEYSPACE statement has two properties: replication and durable_writes.

              CREATE KEYSPACE <identifier> WITH <properties>
              
              CREATE KEYSPACE “KeySpace Name” WITH replication = {'class': ‘Strategy name’, 'replication_factor' : ‘No.Of   replicas’};

              CREATE KEYSPACE “KeySpace Name” WITH replication = {'class': ‘Strategy name’, 'replication_factor' : ‘No.Of  replicas’} AND durable_writes = ‘Boolean value’;

              
Example :   

         CREATE KEYSPACE Vkhan WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};

### Replication strategy in Cassandra
  
         1. SimpleStrategy :It is a simple strategy that is recommended for multiple nodes over multiple racks in a single data center.

         2. LocalStrategy :It is the strategy in which we will use a replication strategy for internal purposes such that is used for system and sys_auth keyspaces are internal  keyspaces. In Cassandra internal keyspaces implicitly handled by Cassandra’s storage architecture for managing authorization and authentication.


         3. NetworkTopologyStrategy : It is the strategy in which we can store multiple copies of data on different data centers as per need. This is one important reason to use NetworkTopologyStrategy when multiple replica nodes need to be placed on different data centers.



### Creating a Table:

                      CREATE (TABLE | COLUMNFAMILY) <tablename>
                      ('<column-definition>' , '<column-definition>')
                      (WITH <option> AND <option>)


A Cassandra column family has the following attributes −

### keys_cached − 

It represents the number of locations to keep cached per SSTable.

### rows_cached − 

It represents the number of rows whose entire contents will be cached in memory.

### preload_row_cache − 
It specifies whether you want to pre-populate the row cache.

### Column
column has three values in Cassandra

- key or column name
- value
- time stamp

### SuperColumn

super column stores a map of sub-columns,super column help to store all column family in same location.

- name [byte]
- cols:map<byte[],column>

----------------------------------------------------------------
### Apache Spark Load data from Hive to Cassandra


              spark-shell \
                  --num-executors 20 \
                  --executor-memory 10g \
                  --driver-memory 10g  \
                  --executor-cores 5  \
                  --driver-cores 5  \
                  --conf spark.executor.memoryOverhead=1024 \
                  --conf spark.hadoop.metastore.catalog.default=hive \
		  --conf spark.geode.locators=127.0.0.1:[10334] \
                  --conf spark.cassandra.connection.host=127.0.0.1 \
                  --conf spark.cassandra.auth.username=cassandra \
                  --conf spark.cassandra.auth.password=cassandra \
                  --conf spark.cassandra.connection.port=9042 \
                  --jars /home/vaquarkhan/spark-cassandra-connector-2.3.1-s_2.11.jar,/home/vaquarkhan/commons-configuration-1.8.jar
		  
		  ## This is getting jar from maven
		  --packages com.datastax.spark:spark-cassandra-connector-2.3.1-s_2.11.jar 



                 import com.datastax.spark.connector._
		 import org.apache.spark.SparkContext
		 import org.apache.spark.SparkContext._
		 import org.apache.spark.SparkConf
		 import org.apache.spark.sql.cassandra._
		 import org.apache.commons.configuration._
		 import org.apache.spark.sql.hive.HiveContext
		 import spark.implicits._

			 //Hive
			  var df = spark.sql("select * from emp LIMIT 100  ")
			  
			  df.count
                               //Cassandra doc solution not good to use so commented 
			//sc.stop
			//Not needed to create separate contxt for cassandra
			// val sparkConf = new SparkConf()
			//        .setAppName("Cassandra")
			//		.setMaster("local")
			//		.set("spark.hadoop.metastore.catalog.default","hive")
			//		.set("spark.cassandra.connection.host", "127.0.0.1")
			//		.set("spark.cassandra.connection.port","port")
			//		.set("spark.cassandra.auth.username", "username")
			//		.set("spark.cassandra.auth.password", "password") 
			// val sc = new SparkContext(sparkConf)
		  
		  	//save into cassandra
		        df.write.format("org.apache.spark.sql.cassandra").options(Map( "table" -> "identities", "keyspace" -> "edlapi")).save()

			 val table = sc.cassandraTable("tablespace", "table")
		  
			 table.count
			 
----------------------------------------------------------------

### Read from HIVE and save
	  var df = spark.sql("select * from emp LIMIT 100  ")

          df.write.format("org.apache.spark.sql.cassandra").options(Map( "table" -> "table", "keyspace" -> "keyspace")).save()

         import org.apache.spark.sql.SaveMode._ 
	
	  //append
          df.write
            .format("org.apache.spark.sql.cassandra")
            .options(Map("table" -> "table", "keyspace" -> "keyspace"))
            .mode(org.apache.spark.sql.SaveMode.Append)
            .save()
	    
	  //override
	   df.write.
	  format("org.apache.spark.sql.cassandra")
	  .options(Map("table" -> "table", "keyspace" -> "keyspace"))
	  .mode(org.apache.spark.sql.SaveMode.Overwrite)
	  .option("confirm.truncate","true")
	  .save()
   




-----------------------------------------------------------------


### SaveMode.Append

Append mode means that when saving a DataFrame to a data source, if data/table already exists, contents of the DataFrame are expected to be appended to existing data.

### SaveMode.ErrorIfExists

ErrorIfExists mode means that when saving a DataFrame to a data source, if data already exists, an exception is expected to be thrown.

### SaveMode.Ignore

Ignore mode means that when saving a DataFrame to a data source, if data already exists, the save operation is expected to not save the contents of the DataFrame and to not change the existing data.

### SaveMode.Overwrite

Overwrite mode means that when saving a DataFrame to a data source, if data/table already exists, existing data is expected to be overwritten by the contents of the DataFrame.

Note: If we are using mode(SaveMode.Overwrite) then we should use tableProperties.put("confirm.truncate", "true"); otherwise we will get error message.

- https://gist.github.com/kyrsideris/78c8c0ca73a72a92f009dcb540f0e1df
---------------------------------------------------------------
### Read Python:

				>>> from pyspark import SparkContext, SparkConf
				>>> from pyspark.sql import SQLContext, SparkSession
				>>> from pyspark.sql.types import *
				>>> import os
				>>> spark = SparkSession.builder \
				...   .appName('SparkCassandraApp') \
				...   .config('spark.cassandra.connection.host', '127.0.0.1') \
				...   .config('spark.cassandra.connection.port', '9042') \
				...   .config('spark.cassandra.output.consistency.level','ONE') \
				...   .master('local[2]') \
				...   .getOrCreate()
				>>> df = spark.read.format("org.apache.spark.sql.cassandra").options(table="emp",keyspace="tutorialspoint").load()
				>>> df.show()
				+------+---------+--------+----------+-------+
				|emp_id| emp_city|emp_name| emp_phone|emp_sal|
				+------+---------+--------+----------+-------+
				|     2|Bhopal   |  Vaquar|9848022339|  40000|
				|     1|Hyderabad|     ram|9848022338|  50000|
				|     3|  Chennai|  Zidan |9848022330|  45000|
				+------+---------+--------+----------+-------+
				
				

- https://jasset75.github.io/Spark-Cassandra-Notes/Examples/dataset-join-02.html		
----------------------------------------------------------------
- https://alexott.blogspot.com/2020/07/spark-effective-joins-with-cassandra.html
- https://www.datastax.com/blog/2012/02/schema-cassandra-11
- https://pkghosh.wordpress.com/2011/03/02/cassandra-secondary-index-patterns/
- https://tech.ebayinc.com/engineering/cassandra-data-modeling-best-practices-part-1/
- https://tech.ebayinc.com/engineering/cassandra-data-modeling-best-practices-part-2/
- https://www.datastax.com/blog/2015/02/basic-rules-cassandra-data-modeling
- https://intellipaat.com/blog/tutorial/cassandra-tutorial/non-relational-cassandra-data-model/
- https://www.youtube.com/watch?v=n-BXDdum0Kk
- https://stackoverflow.com/questions/41844770/understanding-cassandra-data-model
- https://stackoverflow.com/questions/3245267/column-family-concept-and-data-model
- https://stackoverflow.com/questions/41844770/understanding-cassandra-data-model
- https://www.javahotchocolate.com/notes/cassandra.html
- https://www.youtube.com/watch?v=Mvsy2DZhKSw
- https://github.com/danielschulz/Cassandra-Cheat-Sheet/blob/master/Cassandra-Cheat-Sheets.pdf
- https://github.com/cr3ativ3-d3v3lop3r/cassandra-cheat-sheet
- https://github.com/sunilsoni/Cassandra-Data-Modeling
- https://gist.github.com/korya/706a59a63cf9a4238674
- https://github.com/Anant/awesome-cassandra
- https://github.com/datastax/management-api-for-apache-cassandra
- https://github.com/allegro/cassandra-modeling-kata
- https://github.com/apache/cassandra
- https://www.tutorialspoint.com/cassandra/cassandra_data_model.htm
- https://mattilyra.github.io/2016/05/04/spark-shell-cassandra-connection.html
- https://shekharsingh.com/blog/2017/01/24/processing-cassandra-data-with-apache-spark-part-2.html
- https://www.aegissofttech.com/articles/tutorial-spark-cassandra-datastax.html
- https://github.com/bezkoder/spring-boot-data-cassandra


### Connect Squirrel SQL with Cassnadra
- https://dbschema.com/download.html
- https://gist.github.com/attila123/d52d521a0f25fc912ec86a77c7c0a30d
-------------------------

### MYSQL

       df.write.format("jdbc")
	      .option("batchsize", 100000)
	.option("truncate", "true")
	.option("url", "jdbc:mysql://localhost:3307/schema?          useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC&user=xxx&password=XXX")
	 .option("driver", "com.mysql.jdbc.Driver")
	 .option("dbtable", "table")
	 .mode("overwrite")
	 .save()



###  Cassandra vs Mango vs Hbase vs Write workload
- https://www.datastax.com/wp-content/uploads/2013/02/WP-Benchmarking-Top-NoSQL-Databases.pdf

 ![Alt Text](https://i.stack.imgur.com/ZlfXZ.png )


### Load process
For load, Couchbase, HBase, and MongoDB all had to be configured for non-durable writes to complete in a reasonable amount of time, with Cassandra being the only database performing durable write operations. Therefore, the numbers below for Couchbase, HBase, and MongoDB represent non-durable write metrics.

 ![Alt Text](https://www.datastax.com/sites/default/files/inline-images/chart-load-process-v4-rev2.png )
 
- https://www.datastax.com/products/compare/nosql-performance-benchmarks


###  Cassandra vs Mysql


![Alt Text](https://lh3.googleusercontent.com/proxy/gZD5zMj1LVulUttMB-WXTwT2Jx-_e36WLVIhls2Izjlt17e_i6eqB4hny0chg7Ew5p3V7bCLVFDZDkVNmdOObmz6ho10aSVvupJBYZ7Qn1g )
- https://adataanalyst.com/data-analysis-resources/a-comparison-between-cassandra-and-mysql/
- 

### Hbase vs Cassandra 
![Alt Text](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRLSFp5vMGL-gjeLCojqqTvnD7mxsLX2X01uQ&usqp=CAU )

- https://www.datastax.com/blog/cassandra-architecture-and-performance-mid-2014

Single node performance (ops/s) of the most popular NoSQL databases performing synchronously durable writes

 ![Alt Text](https://www.datastax.com/sites/default/files/inline-images/single-node-ycsb-637_0.png )
