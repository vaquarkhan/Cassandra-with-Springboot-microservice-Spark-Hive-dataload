# Cassandra-with-Springboot
Cassandra-with-Springboot -notes 


### Column family:
A column family is a database object that contains columns of related data. It is a tuple (pair) that consists of a key-value pair, where the key is mapped to a value that is a set of columns. In analogy with relational databases, a column family is as a "table", each key-value pair being a "row". Each column is a tuple (triplet) consisting of a column name, a value, and a timestamp. In a relational database table, this data would be grouped together within a table with other non-related data.

![Alt Text](https://i.stack.imgur.com/rPdYd.jpg )


          Book {

           key: 8812244567{ name: “Hadoop The Definitive Guide”, author:” Tom White”, publisher:”Oreilly”, priceInr;650, category: “hadoop”, edition:4},

           key: 8812244568{ name”” Hadoop in Action”, author: “Chuck Lam”, publisher:”manning”, priceInr;590, category: “hadoop”},

           key: 8812244569{ name:” Cassandra: The Definitive Guide”, author: “Eben Hewitt”, publisher:” Oreilly”, priceInr:600, category: “cassandra”},

          }




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
		  
			 val table = sc.cassandraTable("tablespace", "table")
		  
			 table.count
		
----------------------------------------------------------------
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
