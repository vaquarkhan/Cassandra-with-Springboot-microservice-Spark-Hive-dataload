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




----------------------------------------------------------------


- https://tech.ebayinc.com/engineering/cassandra-data-modeling-best-practices-part-1/
- https://tech.ebayinc.com/engineering/cassandra-data-modeling-best-practices-part-2/
- https://www.datastax.com/blog/2015/02/basic-rules-cassandra-data-modeling
- https://www.youtube.com/watch?v=n-BXDdum0Kk
- https://stackoverflow.com/questions/41844770/understanding-cassandra-data-model
- https://stackoverflow.com/questions/3245267/column-family-concept-and-data-model
- https://stackoverflow.com/questions/41844770/understanding-cassandra-data-model
- https://www.javahotchocolate.com/notes/cassandra.html
- https://www.youtube.com/watch?v=Mvsy2DZhKSw
