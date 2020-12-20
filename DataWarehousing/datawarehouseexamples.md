## DWH - SCD1

```sql
CREATE TABLE customers (
  customer_id      NUMBER        GENERATED ALWAYS AS IDENTITY,
  customer_name   VARCHAR2(255) NOT NULL,
  customer_address VARCHAR2(255) ,
  customer_phone_no NUMBER
);

insert into customers
(customer_name,customer_address,customer_phone_no)
values
('Aditya','Mapsko',1);

insert into customers
(customer_name,customer_address,customer_phone_no)
values
('Sakshi','Sector-17',2);

CREATE TABLE cust_stg (
  customer_name   VARCHAR2(255) NOT NULL,
  customer_address VARCHAR2(255) ,
  customer_phone_no NUMBER
);

insert into cust_stg
(customer_name,customer_address,customer_phone_no)
values
('Sakshi','Mapsko',3);

insert into cust_stg
(customer_name,customer_address,customer_phone_no)
values
('Lucky','Mapsko',5);
```

<details>
  <summary>Answer</summary>
  
```sql
merge into customers c1
using cust_stg stg
on (c1.customer_name = stg.customer_name)
when matched then 
update set c1.customer_address = stg.customer_address,
           c1.customer_phone_no = stg.customer_phone_no
when not matched then
insert (customer_name, customer_address, customer_phone_no) 
values (stg.customer_name, stg.customer_address, stg.customer_phone_no);
```

```sql

create unique index cust_stg_uq on cust_stg(customer_name);

update 
  (select c1.customer_address AS new_customer_address, c1.
  AS new_customer_phone_no, 
          stg.customer_address AS old_customer_address, stg.customer_phone_no AS old_customer_phone_no
   from customers c1 join cust_stg stg on c1.customer_name = stg.customer_name) cust
set cust.new_customer_address = cust.old_customer_address,
    cust.new_customer_phone_no = cust.old_customer_phone_no;
    
insert into customers(customer_name,customer_address, customer_phone_no)
(select stg.customer_name, stg.customer_address, stg.customer_phone_no
from cust_stg stg left outer join customers c1 
on  stg.customer_name  = c1.customer_name
where c1.customer_name is  null)
```

</details> 

## DWH - SCD2

```sql
CREATE TABLE customers (
  customer_id      NUMBER        GENERATED ALWAYS AS IDENTITY,
  customer_name   VARCHAR2(255) NOT NULL,
  customer_address VARCHAR2(255) ,
  customer_phone_no NUMBER,
  effective_Date date ,
  expiration_date date
);

insert into customers
(customer_name,customer_address,customer_phone_no,effective_Date,expiration_date)
values
('Aditya','Mapsko',1,to_Date('20190101','YYYYMMDD'),to_date('99991231','YYYYMMDD'));

insert into customers
(customer_name,customer_address,customer_phone_no,effective_Date,expiration_date)
values
('Sakshi','Sector-17',2,to_Date('20180101','YYYYMMDD'),to_date('99991231','YYYYMMDD'));

CREATE TABLE cust_stg (
  customer_name   VARCHAR2(255) NOT NULL,
  customer_address VARCHAR2(255) ,
  customer_phone_no NUMBER,
  update_date DATE
);

insert into cust_stg
(customer_name,customer_address,customer_phone_no,update_Date)
values
('Sakshi','Mapsko',3,to_Date('20190201','YYYYMMDD'));

insert into cust_stg
(customer_name,customer_address,customer_phone_no,update_Date)
values
('Lucky','Mapsko',5,to_Date('20200101','YYYYMMDD'));

```
<details>
  <summary>Answer</summary>
  
```sql

create unique index cust_stg_uq on cust_stg(customer_name);

insert into customers(customer_name, customer_address, customer_phone_no, effective_date, expiration_date)
select stg.customer_name, stg.customer_address, stg.customer_phone_no, stg.update_date, '31-DEC-99'
from cust_stg stg left outer join customers cust on stg.customer_name = cust.customer_name;

update 
(select cust.effective_date AS old_effective_date, cust.expiration_date AS old_expiration_date,
        stg.update_date AS new_effective_date
from customers cust join cust_stg stg on cust.customer_name = stg.customer_name 
and  cust.effective_date <> stg.update_date                       
) s1
set s1.old_expiration_date = s1.new_effective_date;

merge into customers cust
using (select c1.customer_name, c1.customer_address, c1.customer_phone_no, 
         c1.effective_date AS effective_date, cs.update_date
       from customers c1 join cust_stg cs on c1.customer_name = cs.customer_name
       union all
       select customer_name, customer_address, customer_phone_no, 
       update_date AS effective_date, update_date from cust_stg) stg
on (cust.customer_name = stg.customer_name and cust.effective_date = stg.effective_date)
when matched then update
set cust.expiration_date = stg.update_date
when not matched then 
insert(customer_name, customer_address, customer_phone_no, effective_date, expiration_date) 
values(stg.customer_name, stg.customer_address, stg.customer_phone_no, stg.update_date, '31-DEC-9999');

```

</details>

## DWH - FACT Load

```sql

create table fund(
fund_id number,
fund_name varchar(50),
ph_fund_id number
);

create table security_master(
security_id number,
security_type varchar(20),
effective_date date,
expiration_date date,
mdm_security_id number
);

create table date_dim(
date_dim_id number,
effective_date date,
week_num number,
month_num number,
year_num number
);


create table holding_stg(
ph_fund_id number,
mdm_security_id number,
effective_date date,
traded_value number,
market_value number,
process_date date
);

create table holding(
fund_id number,
ph_fund_id number,
security_id number,
mdm_security_id number,
effective_date date,
traded_value number,
market_value number,
process_date date
);


insert into fund values(1, 'Fidelity Fund', 11);
insert into fund values(2, 'TCS Fund A', 22);
insert into fund values(3, 'YES Bank Fund', 33);
insert into fund values(4, 'HDFC Fund', 44);
insert into fund values(5, 'ICICI Fund', 55);


insert into security_master values(1000, 'Cash', to_date('10-JAN-2020','DD-MON-YYYY'), to_date('31-DEC-9999','DD-MON-YYYY'), 10110);
insert into security_master values(1001, 'Equity', to_date('10-JAN-2020','DD-MON-YYYY'), to_date('10-APR-2020','DD-MON-YYYY'), 10111);
insert into security_master values(1002, 'Bond', to_date('01-APR-2020','DD-MON-YYYY'), to_date('31-DEC-9999','DD-MON-YYYY'), 10112);
insert into security_master values(1003, 'MultiAsset', to_date('01-JAN-2020','DD-MON-YYYY'), to_date('31-DEC-9999','DD-MON-YYYY'), 10113);
insert into security_master values(1004, 'Gold', to_date('20-FEB-2020','DD-MON-YYYY'), to_date('20-MAR-2020','DD-MON-YYYY'), 10114);
insert into security_master values(1005, 'Equity', to_date('11-APR-2020','DD-MON-YYYY'), to_date('31-DEC-9999','DD-MON-YYYY'), 10111);
insert into security_master values(1006, 'Gold', to_date('21-MAR-2020','DD-MON-YYYY'), to_date('31-DEC-9999','DD-MON-YYYY'), 10114);


insert into date_dim values(20200101, to_date('01-JAN-2020','DD-MON-YYYY'), 1, 1, 2020);
insert into date_dim values(20200110, to_date('10-JAN-2020','DD-MON-YYYY'), 2, 1, 2020);
insert into date_dim values(20200220, to_date('20-FEB-2020','DD-MON-YYYY'), 8, 2, 2020);
insert into date_dim values(20200320, to_date('20-MAR-2020','DD-MON-YYYY'), 12, 3, 2020);
insert into date_dim values(20200401, to_date('01-APR-2020','DD-MON-YYYY'), 14, 4, 2020);
insert into date_dim values(20200410, to_date('10-APR-2020','DD-MON-YYYY'), 15, 4, 2020);


insert into holding_stg values(11, 10110, to_date('10-JAN-2020','DD-MON-YYYY'),1000, 1500, to_date('11-JAN-2020','DD-MON-YYYY'));  
insert into holding_stg values(22, 10111, to_date('01-APR-2020','DD-MON-YYYY'),2000, 2500, to_date('02-APR-2020','DD-MON-YYYY'));  
insert into holding_stg values(33, 10113, to_date('10-JAN-2020','DD-MON-YYYY'),1500, 3000, to_date('11-JAN-2020','DD-MON-YYYY'));  
--insert into holding_stg values(11, 10110, to_date('10-JAN-2020','DD-MON-YYYY'),1000, 1000, to_date('11-JAN-2020','DD-MON-YYYY'));  
--insert into holding_stg values(11, 10110, to_date('10-JAN-2020','DD-MON-YYYY'),1000, 1000, to_date('11-JAN-2020','DD-MON-YYYY'));  

select * from fund;
select * from security_master ;
select * from date_dim;
select * from holding_stg;
select * from holding;

```

<details>
  <summary>Answer</summary>

```sql

insert into holding(fund_id, ph_fund_id, security_id, mdm_security_id, effective_date, traded_value, market_value, process_date)
select f.fund_id, stg.ph_fund_id, sm.security_id, stg.mdm_security_id, stg.effective_date, stg.traded_value, stg.market_value, stg.process_date 
from holding_stg stg 
left outer join fund f on stg.ph_fund_id = f.ph_fund_id
left outer join security_master sm on stg.mdm_security_id = sm.mdm_security_id 
and stg.effective_date between sm.effective_date and sm.expiration_date;

```

</details>

## DWH - Reporting Query

<details>
  <summary>Answer</summary>

```sql

select f.fund_name, hld.* 
from holding hld join fund f on f.fund_id = hld.fund_id
join security_master sm on sm.security_id = hld.security_id
join date_dim dd on hld.effective_date = dd.effective_date
where dd.month_num = 1

```
</details>

## Backfill data like files missing on 12th process date after 10 days.   

<details>
  <summary>Answer</summary>
  
1. Insert those records which are received only on 12th process date
2. Update those records for which latest was received on 12th only.

</details>

<details>
  <summary>Answer</summary>

```sql
10 JAN
11 JAN
----------   12 JAN                      
13 JAN                                   
14 JAN                                    
15 JAN
16 JAN
17 JAN 
18 JAN

FACT -- DIRECTLY INSERT THAT SNAPSHOT

ORDER DATE     PROCESSING DAY
11 JAN         11 JAN
-------------- 12 JAN  
11 JAN         14 JAN

DIMENSION -

12 -- NEVER CAME LATER
12 --- ALREADY CAME

STG

FUND_ID  PROCESSING_DAY   
1        12 JAN
2        12 JAN  
3        12 JAN

MAIN

FUND_ID  PROCESSING_DAY

1        18 JAN 
2        11 JAN          
4        11 JAN         
5        13 JAN 

UPDATE MAIN M1
SELECT 
FROM STG M1 JOIN MAIN M2 ON M1.FUND_ID = M2.FUND_ID
WHERE STG.PROCESSING_DAY > MAIN.PROCESSING_DAY

INSERT INTO MAIN M1
SELECT 
FROM STG M1 LEFT OUTER JOIN MAIN M2
WHERE M2.FUND_ID IS NULL
```
</details>

## How to handle late arriving dimensions ?

<details>
<summary>Answer</summary>
Derive dimensional context and then load fact table and then later update when dimensional data comes. 
</details>

## How to reload corrupt data as quickly as possible ?

<details>
<summary>Answer</summary>
By using audit dimension and delete them .
</details>


## Incremental data load

<details>
<summary>Answer</summary>
 
Use audit dimension or hash values to compare data.

```sql
create table src_tbl
( srcid number,
  srcdate date
);

create table log_tbl
(logid number,
jobid number,
status varchar(10),
startdate date,
enddate date,
cutoff date
);

create table job
(jobid number,
jobcode varchar2(20)
);

create table stg as select * from src_tbl where 1=2;

insert into src_tbl values(123, to_date('20-APR-2020','DD-MON-YYYY'));
insert into src_tbl values(456, to_date('21-APR-2020','DD-MON-YYYY'));
insert into src_tbl values(789, to_date('22-APR-2020','DD-MON-YYYY'));
insert into src_tbl values(789, to_date('23-APR-2020','DD-MON-YYYY'));


insert into log_tbl values(1,1,'OK','20-APR-2020','20-APR-2020','20-APR-2020');
insert into log_tbl values(2,2,'OK','20-APR-2020','20-APR-2020','20-APR-2020');
insert into log_tbl values(3,1,'OK','21-APR-2020','21-APR-2020','21-APR-2020');
insert into log_tbl values(4,1,'NOTOK','22-APR-2020','22-APR-2020','22-APR-2020');
insert into log_tbl values(5,1,'OK','22-APR-2020','22-APR-2020','22-APR-2020');


insert into job values(1, 'DBLOAD');
insert into job values(2, 'DBREAD');


insert into stg
select * from src_tbl where srcdate > 
(select cutoff from log_tbl
where logid = (select max(logid) from log_tbl where status ='OK' 
               and jobid = (select jobid from job where jobcode = 'DBLOAD'))
);

insert into log_tbl values ((select max(logid) from log_tbl), 1, 'OK', SYSDATE, SYSDATE, NULL);

update log_tbl
set cutoff = (select max(srcdate)from stg)
where jobid = (select jobid from job where jobcode = 'DBLOAD') and cutoff IS NUll;

```

</details>

<details>
<summary>Answer2</summary>

```sql

*Orders table: *ORDER_ID | CUSTOMER_ID | ORDER_STATUS | ORDER_DATE | DELIVERY_ADDRESS

*Order Items table:* ORDER_ITEM_ID | ORDER_ID | QUANTITY | UNIT_COST | TAX

*Denormalised table:*
ORDER_ID | ORDER_DATE| ORDER_ITEM_ID | CUSTOMER_ID | ORDER_STATUS | QUANTITY | SALE_AMOUNT | DELIVERY_ADDRESS

```
```sql

create table orders
(order_id number,
customer_id number,
order_status varchar2(20),
order_date date,
delivery_address varchar2(100),
processing_day date
);

create table order_item
(
order_item_id number,
order_id number,
quantity number,
unit_cost number,
tax number,
processing_day date
);

create table order_den_tbl
(
order_id number,
order_date date, 
order_item_id number, 
customer_id number,
order_status varchar2(20),
quantity number,
sale_amount number, 
delivery_address varchar2(100),
processing_day date
);


insert into orders values (1, 1, 'Shipped', '20-APR-2020', 'London','24-APR-2020');
insert into orders values (2, 1, 'Delivered', '10-APR-2020', 'Glassgow','24-APR-2020');
insert into orders values (3, 2, 'Shipped', '21-APR-2020', 'Toronto','24-APR-2020');
insert into orders values (4, 3, 'Packed', '22-APR-2020', 'Delhi','24-APR-2020');
insert into orders values (5, 4, 'Delivered', '20-APR-2020', 'Mumbai','24-APR-2020');

insert into order_item values(1, 1, 5, 100, 100,'24-APR-2020');
insert into order_item values(2, 1, 1, 200, 100,'24-APR-2020');
insert into order_item values(3, 1, 2, 150, 200,'24-APR-2020');
insert into order_item values(1, 2, 1, 100, 50,'24-APR-2020');
insert into order_item values(2, 2, 4, 100, 50,'24-APR-2020');
insert into order_item values(1, 3, 2, 200, 100,'24-APR-2020');
insert into order_item values(2, 3, 3, 200, 100,'24-APR-2020');
insert into order_item values(1, 4, 1, 500, 100,'24-APR-2020');
insert into order_item values(1, 5, 2, 500, 200,'24-APR-2020');


create table job_detail
(jobid number,
jobname varchar2(20)
);

insert into job_detail values(1, 'ORDERLOAD');
insert into job_detail values(2, 'ORDERITEMLOAD');
insert into job_detail values(3, 'ORDERDELETE');
insert into job_detail values(4, 'ORDERITEMDELETE');
insert into job_detail values(5, 'ORDERDENOMLOAD');


create table job_log_status
(
logid number,
jobid number,
status varchar2(20),
run_date date,
start_date date,
end_date date
);


insert into job_log_status values (1, 1, 'ENDEDOK', '23-APR-2020','24-APR-2020','24-APR-2020');
insert into job_log_status values (2, 2, 'ENDEDOK', '23-APR-2020','24-APR-2020','24-APR-2020');

#Solution1

merge into order_den_tbl od
using(
select o.order_id, o.order_date, oi.order_item_id, o.customer_id, o.order_status, oi.quantity,
(oi.unit_cost * oi.quantity ) + oi.tax as sale_amount, o.delivery_address, 
o.processing_day as order_process_date, oi.processing_day as processing_day
from orders o join order_item oi on o.order_id = oi.order_id
) stg
on (od.order_id = stg.order_id and od.order_date = stg.order_date 
    and od.order_item_id = stg.order_item_id and od.customer_id = stg.customer_id)
when matched then update
  set od.order_status = stg.order_status,
      od.quantity     = stg.quantity,
      od.sale_amount  = stg.sale_amount,
      od.delivery_address = stg.delivery_address,
      od.processing_day   = stg.processing_day
when not matched then 
insert(order_id, order_date, order_item_id, customer_id, order_status, quantity, sale_amount, delivery_address,processing_day)
values(stg.order_id, stg.order_date, stg.order_item_id, stg.customer_id, stg.order_status, 
       stg.quantity, stg.sale_amount, stg.delivery_address,stg.processing_day);
       
insert into job_log_status values ((select max(logid)+1 from job_log_status), 
    (select jobid from job_detail where jobname = 'ORDERDENOMLOAD'), 'ENDEDOK', 
    (select max(processing_day) from order_den_tbl),SYSDATE,SYSDATE);

#Solution 2

update orders set order_status = 'Delivered' where order_id = 1;
update order_item set quantity = 1 where order_id = 1 and order_item_id = 3;
update order_item set unit_cost = 300 where order_id = 2 and order_item_id = 1;
       
create unique index order_uq on orders(order_id);
create unique index order_item_uq on order_item(order_id, order_item_id);

select max_process_date from 
(
select max(processing_day) as max_process_date from orders
union
select max(processing_day) from order_item
);

insert into order_den_tbl
select o.order_id, o.order_date, oi.order_item_id, o.customer_id, o.order_status, oi.quantity,
(oi.unit_cost * oi.quantity ) + oi.tax as sale_amount, o.delivery_address, max_process_date as processing_day
from orders o join order_item oi 
on o.order_id = oi.order_id
and ( o.processing_day > (select run_date from job_log_status where logid = (select max(logid) from job_log_status where jobid = 5) )
   or od.processing_day > (select run_date from job_log_status where logid = (select max(logid) from job_log_status where jobid = 5) )
);

insert into job_log_status 
values( (select max(logid)+1 from job_log_status), 
  (select jobid from job_detail where jobname = 'ORDERDENOMINS'), 
'ENDEDOK', (select max(processing_day) from order_den_tbl),SYSDATE,SYSDATE);


update 
(select od.order_status as old_order_status, o.order_status as new_order_status
from orders o join order_den_tbl od 
on o.order_id = od.order_id
--where od.order_status <> o.order_status 
where od.processing_day > (select run_date from job_log_status 
  where logid = (select max(logid) from job_log_status where jobid = 6) ) 
) vw
set vw.old_order_status = vw.new_order_status; 


update
(
select od.quantity as old_quantity, oi.quantity as new_quantity,
  od.sale_amount as old_sale_amount, (oi.quantity * oi.unit_cost ) + oi.tax as new_sale_amount
from order_item oi join order_den_tbl od 
on oi.order_id = od.order_id
and oi.order_item_id = od.order_item_id
--where (od.quantity <> oi.quantity OR od.sale_amount <> ( (oi.quantity * oi.unit_cost ) + oi.tax ) ) 
where od.processing_day > (select run_date from job_log_status 
  where logid = (select max(logid) from job_log_status where jobid = 6) ) 
) vw
set vw.old_quantity = vw.new_quantity,
    vw.old_sale_amount = vw.new_sale_amount;
    
insert into job_log_status 
values( (select max(logid)+1 from job_log_status), 
  (select jobid from job_detail where jobname = 'ORDERDENOMUPDT'), 
'ENDEDOK', (select max(processing_day) from order_den_tbl),SYSDATE,SYSDATE);
    
```

</details>
