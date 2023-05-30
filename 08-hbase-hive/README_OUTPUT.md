==========HIVE==========


******************* I *******************

hive> create table customer_info (custID int, custName string, orderID int)
    > row format delimited
    > fields terminated by ','
    > stored as textfile;
OK
Time taken: 0.67 seconds



hive> create table order_info(orderID int, itemID int, quantity int, country string)
    > row format delimited
    > fields terminated by ','
    > stored as textfile;
OK
Time taken: 0.135 seconds


hive> create table item_info(itemID int, itemName string, itemPrice int)
    > row format delimited
    > fields terminated by ','
    > stored as textfile;
OK



hive> load data local inpath '/home/cloudera/customer_info.csv' into table customer_info;
Loading data to table default.customer_info
Table default.customer_info stats: [numFiles=1, totalSize=70]
OK
Time taken: 1.45 seconds


hive> select * from customer_info;
OK
1	sumit	132
2	savani	133
3	gayatri	134
4	onkar	145
5	harshvardhan	146
Time taken: 0.629 seconds, Fetched: 5 row(s)


hive> load data local inpath '/home/cloudera/item_info.csv' into table item_info;
Loading data to table default.item_info
Table default.item_info stats: [numFiles=1, totalSize=38]
OK
Time taken: 0.841 seconds


hive> select * from item_info;
OK
222	bottle	49
223	book	299
224	pen	20
Time taken: 0.155 seconds, Fetched: 3 row(s)


hive> load data local inpath "/home/cloudera/order_info.csv" into table order_info;
Loading data to table default.order_info
Table default.order_info stats: [numFiles=1, totalSize=55]
OK
Time taken: 0.462 seconds


hive> select * from order_info;
OK
132	222	2	india
133	224	50	india
134	224	200	usa
145	223	3	usa
146	224	100	india
Time taken: 0.12 seconds, Fetched: 5 row(s)







******************* J *******************


hive> create index idxName on table customer_info(custID) as 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler' with deferred rebuild;
OK
Time taken: 0.567 seconds

hive> show index on customer_info;
OK
idxname             	customer_info       	custid              	default__customer_info_idxname__	compact             	
Time taken: 0.251 seconds, Fetched: 1 row(s)





******************* K *******************



hive> select sum(order_info.quantity * item_info.itemPrice), avg(order_info.quantity * item_info.itemPrice) from order_info join item_info on order_info.itemID=item_info.itemID;
Query ID = cloudera_20230530071717_68d421d9-b439-43da-bc02-817112265f70
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230530071717_68d421d9-b439-43da-bc02-817112265f70.log
2023-05-30 07:18:02	Starting to launch local task to process map join;	maximum memory = 1013645312
2023-05-30 07:18:05	Dump the side-table for tag: 1 with group count: 3 into file: file:/tmp/cloudera/b7a67588-fc23-47ee-b03f-a60f83309b5a/hive_2023-05-30_07-17-47_147_1956575841930596126-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile01--.hashtable
2023-05-30 07:18:05	Uploaded 1 File to: file:/tmp/cloudera/b7a67588-fc23-47ee-b03f-a60f83309b5a/hive_2023-05-30_07-17-47_147_1956575841930596126-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile01--.hashtable (325 bytes)
2023-05-30 07:18:05	End of local task; Time Taken: 3.51 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1685455131689_0001, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1685455131689_0001/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1685455131689_0001
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2023-05-30 07:18:33,242 Stage-2 map = 0%,  reduce = 0%
2023-05-30 07:18:51,293 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 2.5 sec
2023-05-30 07:19:10,658 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 4.87 sec
MapReduce Total cumulative CPU time: 4 seconds 870 msec
Ended Job = job_1685455131689_0001
MapReduce Jobs Launched: 
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 4.87 sec   HDFS Read: 12418 HDFS Write: 12 SUCCESS
Total MapReduce CPU Time Spent: 4 seconds 870 msec
OK
7995	1599.0
Time taken: 84.691 seconds, Fetched: 1 row(s)





******************* L *******************


hive> select *, (item_info.itemPrice * order_info.quantity) as total from order_info join item_info on order_info.itemID=item_info.itemID order by total desc limit 1;
Query ID = cloudera_20230530072222_2b8191ab-5ebe-42d0-98e0-13410c7a7165
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230530072222_2b8191ab-5ebe-42d0-98e0-13410c7a7165.log
2023-05-30 07:22:26	Starting to launch local task to process map join;	maximum memory = 1013645312
2023-05-30 07:22:28	Dump the side-table for tag: 1 with group count: 3 into file: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_07-22-13_667_4150177648675198827-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile01--.hashtable
2023-05-30 07:22:28	Uploaded 1 File to: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_07-22-13_667_4150177648675198827-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile01--.hashtable (325 bytes)
2023-05-30 07:22:28	End of local task; Time Taken: 2.59 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1685455131689_0002, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1685455131689_0002/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1685455131689_0002
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2023-05-30 07:22:49,376 Stage-2 map = 0%,  reduce = 0%
2023-05-30 07:23:05,914 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 2.44 sec
2023-05-30 07:23:22,734 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 4.44 sec
MapReduce Total cumulative CPU time: 4 seconds 440 msec
Ended Job = job_1685455131689_0002
MapReduce Jobs Launched: 
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 4.44 sec   HDFS Read: 10799 HDFS Write: 5 SUCCESS
Total MapReduce CPU Time Spent: 4 seconds 440 msec
OK
4000
Time taken: 70.561 seconds, Fetched: 1 row(s)







******************* M *******************

hive> SELECT customer_info.custID, customer_info.orderID, customer_info.custName, SUM(order_info.quantity * item_info.itemPrice) AS total FROM order_info JOIN item_info ON order_info.itemID=item_info.itemID JOIN customer_info ON order_info.orderID=customer_info.orderID GROUP BY customer_info.custID, customer_info.orderID, customer_info.custName ORDER BY total DESC LIMIT 1;
Query ID = cloudera_20230530080000_188f4356-5c2b-4251-998f-a5a8a97a93ba
Total jobs = 2
Execution log at: /tmp/cloudera/cloudera_20230530080000_188f4356-5c2b-4251-998f-a5a8a97a93ba.log
2023-05-30 08:01:08	Starting to launch local task to process map join;	maximum memory = 1013645312
2023-05-30 08:01:11	Dump the side-table for tag: 1 with group count: 5 into file: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_08-00-57_815_4239187297801458662-1/-local-10007/HashTable-Stage-3/MapJoin-mapfile31--.hashtable
2023-05-30 08:01:11	Uploaded 1 File to: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_08-00-57_815_4239187297801458662-1/-local-10007/HashTable-Stage-3/MapJoin-mapfile31--.hashtable (405 bytes)
2023-05-30 08:01:11	Dump the side-table for tag: 1 with group count: 3 into file: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_08-00-57_815_4239187297801458662-1/-local-10007/HashTable-Stage-3/MapJoin-mapfile41--.hashtable
2023-05-30 08:01:11	Uploaded 1 File to: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_08-00-57_815_4239187297801458662-1/-local-10007/HashTable-Stage-3/MapJoin-mapfile41--.hashtable (325 bytes)
2023-05-30 08:01:11	End of local task; Time Taken: 2.988 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1685455131689_0006, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1685455131689_0006/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1685455131689_0006
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 1
2023-05-30 08:01:28,386 Stage-3 map = 0%,  reduce = 0%
2023-05-30 08:01:40,748 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 1.96 sec
2023-05-30 08:01:56,554 Stage-3 map = 100%,  reduce = 100%, Cumulative CPU 3.97 sec
MapReduce Total cumulative CPU time: 3 seconds 970 msec
Ended Job = job_1685455131689_0006
Launching Job 2 out of 2
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1685455131689_0007, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1685455131689_0007/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1685455131689_0007
Hadoop job information for Stage-4: number of mappers: 1; number of reducers: 1
2023-05-30 08:02:13,565 Stage-4 map = 0%,  reduce = 0%
2023-05-30 08:02:25,789 Stage-4 map = 100%,  reduce = 0%, Cumulative CPU 1.22 sec
2023-05-30 08:02:39,090 Stage-4 map = 100%,  reduce = 100%, Cumulative CPU 3.1 sec
MapReduce Total cumulative CPU time: 3 seconds 100 msec
Ended Job = job_1685455131689_0007
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 1  Reduce: 1   Cumulative CPU: 3.97 sec   HDFS Read: 15218 HDFS Write: 249 SUCCESS
Stage-Stage-4: Map: 1  Reduce: 1   Cumulative CPU: 3.1 sec   HDFS Read: 5327 HDFS Write: 19 SUCCESS
Total MapReduce CPU Time Spent: 7 seconds 70 msec
OK
3	134	gayatri	4000
Time taken: 102.456 seconds, Fetched: 1 row(s)







******************* N *******************


hive> select sum(order_info.quantity * item_info.itemPrice) from order_info join item_info on order_info.itemID=item_info.itemID group by order_info.country;
Query ID = cloudera_20230530073232_07c24843-a056-4503-9989-00034931f397
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230530073232_07c24843-a056-4503-9989-00034931f397.log
2023-05-30 07:33:00	Starting to launch local task to process map join;	maximum memory = 1013645312
2023-05-30 07:33:04	Dump the side-table for tag: 1 with group count: 3 into file: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_07-32-49_898_4525205006841461011-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile11--.hashtable
2023-05-30 07:33:04	Uploaded 1 File to: file:/tmp/cloudera/e262da87-a7ac-4077-acc6-9d6148f470d4/hive_2023-05-30_07-32-49_898_4525205006841461011-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile11--.hashtable (325 bytes)
2023-05-30 07:33:04	End of local task; Time Taken: 3.462 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1685455131689_0003, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1685455131689_0003/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1685455131689_0003
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2023-05-30 07:33:21,322 Stage-2 map = 0%,  reduce = 0%
2023-05-30 07:33:35,835 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 2.1 sec
2023-05-30 07:33:50,579 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 4.07 sec
MapReduce Total cumulative CPU time: 4 seconds 70 msec
Ended Job = job_1685455131689_0003
MapReduce Jobs Launched: 
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 4.07 sec   HDFS Read: 12648 HDFS Write: 10 SUCCESS
Total MapReduce CPU Time Spent: 4 seconds 70 msec
OK
3098
4897
Time taken: 62.949 seconds, Fetched: 2 row(s)






=================== HBASE ====================
hbase(main):019:0* create 'customer_info','custID','custName','orderID'
0 row(s) in 0.9480 seconds

=> Hbase::Table - customer_info






=================== HIVE ====================

******************* O *******************

hive> create external table hbase_customer_info (custID int, custName string, orderID int) 
    > stored by 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' 
    > with serdeproperties('hbase.columns.mapping'=':key,custName:custName,orderID:orderID')
    > tblproperties('hbase.table.name'='customer_info');
OK
Time taken: 2.64 seconds
hive> insert into hbase_customer_info select * from customer_info;
Query ID = cloudera_20230530074343_cc3c95e6-ffba-4670-92bd-bf9ca637e03e
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
Starting Job = job_1685455131689_0004, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1685455131689_0004/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1685455131689_0004
Hadoop job information for Stage-0: number of mappers: 1; number of reducers: 0
2023-05-30 07:44:16,361 Stage-0 map = 0%,  reduce = 0%
2023-05-30 07:44:34,526 Stage-0 map = 100%,  reduce = 0%, Cumulative CPU 2.94 sec
MapReduce Total cumulative CPU time: 2 seconds 940 msec
Ended Job = job_1685455131689_0004
MapReduce Jobs Launched: 
Stage-Stage-0: Map: 1   Cumulative CPU: 2.94 sec   HDFS Read: 10680 HDFS Write: 0 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 940 msec
OK
Time taken: 40.942 seconds
hive> select * from hbase_customer_info;
OK
1	sumit	132
2	savani	133
3	gayatri	134
4	onkar	145
5	harshvardhan	146
Time taken: 0.216 seconds, Fetched: 5 row(s)


=================== HBASE ====================
hbase(main):022:0> scan 'customer_info'
ROW                                     COLUMN+CELL                                                                                                      
 1                                      column=custName:custName, timestamp=1685457873493, value=sumit                                                   
 1                                      column=orderID:orderID, timestamp=1685457873493, value=132                                                       
 2                                      column=custName:custName, timestamp=1685457873493, value=savani                                                  
 2                                      column=orderID:orderID, timestamp=1685457873493, value=133                                                       
 3                                      column=custName:custName, timestamp=1685457873493, value=gayatri                                                 
 3                                      column=orderID:orderID, timestamp=1685457873493, value=134                                                       
 4                                      column=custName:custName, timestamp=1685457873493, value=onkar                                                   
 4                                      column=orderID:orderID, timestamp=1685457873493, value=145                                                       
 5                                      column=custName:custName, timestamp=1685457873493, value=harshvardhan                                            
 5                                      column=orderID:orderID, timestamp=1685457873493, value=146                                                       
5 row(s) in 0.1600 seconds
