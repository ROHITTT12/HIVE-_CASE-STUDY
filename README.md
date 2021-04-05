# HIVE-_CASE-STUDY
HIVE DATA  ANALAYSIS

## Data Flow:-

## 1) MySQL Database
How To Create a MySQL Database, Tables and Insert Data:-

CREATE DATABASE – create the database. To use this statement, you need the CREATE privilege for the database.
```sql 
CREATE DATABASE DATA;
```
CREATE TABLE – create the table. You must have the CREATE privilege for the table.
```sql 
CREATE TABLE netflix_movies (show_id STRING,type STRING,title STRING,director STRING,cast STRING,country STRING,date_added STRING,release_year STRING,rating STRING,duration STRING,listed_in STRING,description STRING) ROW FORMAT DELIMITED FIELDS TERMINATED BY ‘\t’ LINES TERMINATED BY ‘\n’ STORED AS TEXTFILE;
```
INSERT – To add/insert data to table i.e. inserts new rows into an existing table.
```sql
LOAD DATA LOCAL INFILE  'c:/tmp/discounts.csv'
INTO TABLE discounts
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

## 2) Sqoop
1. Objectives
As we discussed the complete introduction to Sqoop in our previous article “Apache Sqoop – Hadoop Ecosystem Component”. In this article “Sqoop Architecture and Working”, we will learn about Sqoop Architecture. Also, we will learn to work with Sqoop to understand well. But before Sqoop architecture let’s have a look at Sqoop introduction to brush up your Knowledge.

2. What is Sqoop?
Basically, using RDBMS applications generally interacts with the relational database. Therefore it makes relational databases one of the most important sources that generate Big Data. However, in the relational structures, such data is stored in RDB Servers. Although, by offering feasible interaction between the relational database server and HDFS, Sqoop plays the vital role in Hadoop ecosystem.
To be more specific, it is a tool that aims to transfer data between HDFS (Hadoop storage) and relational database servers. Such as MySQL, Oracle RDB, SQLite, Teradata, Netezza, Postgres and many more. In addition, imports data from relational databases to HDFS. Also, exports data from HDFS to relational databases. Moreover, Sqoop can transfer bulk data efficiently between Hadoop and external data stores. Like as enterprise data warehouses, relational databases, etc.
However, it is very interesting to know that this is how Sqoop got its name.
“SQL to Hadoop & Hadoop to SQL”.
Moreover, to import data from external datastores into Hadoop ecosystem tools we use Sqoop. Such as Hive & HBase.
Since we know what is Apache Sqoop now. Thus, let’s understand Sqoop Architecture and Working now.

Basically, a tool which imports individual tables from RDBMS to HDFS is what we call Sqoop import tool. However, in HDFS we treat each row in a table as a record.
Moreover, our main task gets divided into subtasks, while we submit Sqoop command. However, map task individually handles it internally. On defining map task, it is the subtask that imports part of data to the Hadoop Ecosystem. Likewise, we can say all map tasks import the whole data collectively.

However, Export also works in the same way.
A tool which exports a set of files from HDFS back to an RDBMS is a Sqoop Export tool. Moreover, there are files which behave as input to Sqoop which also contain records. Those files what we call as rows in the table.
Moreover, the job is mapped into map tasks, while we submit our job, that brings the chunk of data from HDFS. Then we export these chunks to a structured data destination. Likewise, we receive the whole data at the destination by combining all these exported chunks of data. However, in most of the cases, it is an RDBMS (MYSQL/Oracle/SQL Server).
In addition, in case of aggregations, we require reducing phase. However, Sqoop does not perform any aggregations it just imports and exports the data. Also, on the basis of the number defined by the user, map job launch multiple mappers.
In addition, each mapper task will be assigned with a part of data to be imported for Sqoop import. Also, to get high-performance Sqoop distributes the input data among the mappers equally. Afterwards, by using JDBC each mapper creates the connection with the database. Also fetches the part of data assigned by Sqoop. Moreover, it writes it into HDFS or Hive or HBase on the basis of arguments provided in the CLI.
1) IMPORT DATA FROM MYSQL TO HDFS
```bash 
sqoop import --connect jdbc:mysql://localhost/sunbeam --username sunbeam --password sunbeam -m1 --table netflix_movies --target-dir /user/sunbeam/sqoop/data --columns show_id,type,title,director,cast,country,date_added,release_year,rating,duration,listed_in,description
```
# 3) hdfs

```sql
hadoop fs -ls hdfs:///user/sunbeam/sqoop/data/
Found 2 items
```
### --part-m-0000 file
### _Sucess file

# 4) hive 

APACHE HIVE TM
The Apache Hive ™ data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage using SQL. Structure can be projected onto data already in storage. A command line tool and JDBC driver are provided to connect users to Hive.

Here, we can see the existence of a default database provided by Hive.

Let's create a new database by using the following command: -

Here, we can see the existence of a default database provided by Hive.

* Let's create a new database by using the following command: -
```sql 
hive> create database demo; 
```
* Let's check the existence of a newly created database.
```sql
show tables;
```
* Hive - Create Table
In Hive, we can create a table by using the conventions similar to the SQL. It supports a wide range of flexibility where the data files for tables are stored. It provides two types of table: -

1) Internal table
2) External table
* Internal Table
The internal tables are also called managed tables as the lifecycle of their data is controlled by the Hive. By default, these tables are stored in a subdirectory under the directory defined by hive.metastore.warehouse.dir (i.e. /user/hive/warehouse). The internal tables are not flexible enough to share with other tools like Pig. If we try to drop the internal table, Hive deletes both table schema and data.

Let's create an internal table by using the following command:-
```sql
hive> create table demo.employee (Id int, Name string , Salary float)  
row format delimited  
fields terminated by ',' ;  
```

The external table allows us to create and access a table and a data externally. The external keyword is used to specify the external table, whereas the location keyword is used to determine the location of loaded data.

As the table is external, the data is not present in the Hive directory. Therefore, if we try to drop the table, the metadata of the table will be deleted, but the data still exists.

To create an external table, follow the below steps: -

Let's create a directory on HDFS by using the following command: -
hdfs dfs -mkdir /HiveDirectory  
Now, store the file on the created directory.
* hdfs dfs -put hive/emp_details /HiveDirectory  
* Let's create an external table using the following command: -
```sql
hive> create external table emplist (Id int, Name string , Salary float)  
row format delimited  
 fields terminated by ','   
location '/HiveDirectory';  
```
