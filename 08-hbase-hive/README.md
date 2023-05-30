# 1

## create

    create table customer_info (custID int, custName string, orderID int) row format delimited fields terminated by ',' stored as textfile;

    create table order_info(orderID int, itemID int, quantity int, country string) row format delimited fields terminated by ',' stored as textfile;

    create table item_info(itemID int, itemName string, itemPrice int) row format delimited fields terminated by ',' stored as textfile;


## load

    load data local inpath '/home/cloudera/customer_info.csv' into table customer_info;

    select * from customer_info;



    load data local inpath '/home/cloudera/item_info.csv' into table item_info;

    select * from item_info;



    load data local inpath "/home/cloudera/order_info.csv" into table order_info;

    select * from order_info;



# 2


    create index idxName on table customer_info(custID) as 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler' with deferred rebuild;

    show index on customer_info;



    select sum(order_info.quantity * item_info.itemPrice), avg(order_info.quantity * item_info.itemPrice) from order_info join item_info on order_info.itemID=item_info.itemID;



    select *, (item_info.itemPrice * order_info.quantity) as total from order_info join item_info on order_info.itemID=item_info.itemID order by total desc limit 1;



# 3


    SELECT customer_info.custID, customer_info.orderID, customer_info.custName, SUM(order_info.quantity * item_info.itemPrice) AS total FROM order_info JOIN item_info ON order_info.itemID=item_info.itemID JOIN customer_info ON order_info.orderID=customer_info.orderID GROUP BY customer_info.custID, customer_info.orderID, customer_info.custName ORDER BY total DESC LIMIT 1;



# 4

    select sum(order_info.quantity * item_info.itemPrice) from order_info join item_info on order_info.itemID=item_info.itemID group by order_info.country;




# 5

    create external table hbase_customer_info (custID int, custName string, orderID int)  stored by 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' with serdeproperties('hbase.columns.mapping'=':key,custName:custName,orderID:orderID') tblproperties('hbase.table.name'='customer_info');




















# ==============HBASE====================


    create 'customer_info','custID','custName','orderID'

    scan 'customer_info'