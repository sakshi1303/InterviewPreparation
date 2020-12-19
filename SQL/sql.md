## Distance between two cities

```sql
drop table dist ;
create table dist (src varchar2(50), dest varchar2(50), distance number);

insert into dist values ('DEL','KOL',700);
insert into dist values ('KOL','DEL',700);
insert into dist values ('MUM','DEL',800);
insert into dist values ('DEL','MUM',800);
insert into dist values ('BAN','DEL',780);
```
#### Duplicate Cities

<details>
<summary>Answer</summary>

```sql
select  d1.src, d1.dest 
from dist d1 left outer join dist d2 
               on d1.src = d2.dest 
              and d1.dest = d2.src
where d1.src <= d2.src;
```
```sql
select  distinct d1.src, d1.dest
from dist d1 join dist d2 
on d1.src = d2.dest
where d1.src < d2.src
```
</details>

#### Distinct Cities

<details>
<summary>Answer</summary>

```sql
select  d1.src, d1.dest 
from dist d1 left outer join dist d2 
               on d1.src = d2.dest 
              and d1.dest = d2.src
where d1.src <= NVL(d2.src, d1.src);
```
</details>

## Sequence of numbers

<details>
<summary>Answer</summary>
  
```sql
with t(n) as 
(select 1 as n from dual
union all
select n+1 as n from t where n < 25)
select * from t ;
```
</details>

## Employee timesheets

#### No of employees inside the office

```sql
DROP TABLE TIMESHEETS;
CREATE TABLE TIMESHEETS(empid NUMBER , timestamps NUMBER , in_out VARCHAR2(50));

insert into timesheets values(1,830,'IN');
insert into timesheets values(2,900,'IN');
insert into timesheets values(3,930,'IN');
insert into timesheets values(1,945,'OUT');
insert into timesheets values(2,946,'OUT');
insert into timesheets values(4,1000,'IN');
```

<details>
<summary>Answer</summary>

```sql
select count(*) from 
(select dense_rank() over(partition by empid order by timestamps desc) rn , in_out from timesheets t)
where in_out='IN' and rn=1 ;
```
</details>

#### Total time spent by employees inside office

<details>
<summary>Answer</summary>

```sql
select sum(time_diff), empid 
from
(select 
lead(timestamps) over(partition by empid order by timestamps) - timestamps time_diff , empid , in_out from timesheets)
where in_out='IN'
group by empid ;
```
</details>

## Batsman Outscoring Himself

```sql
drop table ipl_batsman_score ;
create table ipl_batsman_score
(batsman_id number , match_id number, score number);

insert all
into ipl_batsman_score values (1,1,10)
into ipl_batsman_score values (1,2,15)
into ipl_batsman_score values (1,3,100)
into ipl_batsman_score values (2,1,101)
into ipl_batsman_score values (2,2,115)
into ipl_batsman_score values (2,3,90)
into ipl_batsman_score values (1,4,45)
into ipl_batsman_score values (1,5,155)
into ipl_batsman_score values (1,6,60)
into ipl_batsman_score values (2,4,10)
into ipl_batsman_score values (2,5,20)
into ipl_batsman_score values (2,6,100)
select 1 from dual;
```
<details>
<summary>Answer</summary>

```sql
select batsman_id, count(*)
from
(select batsman_id, match_id, score , rank() over (partition by batsman_id order by score) rn
from ipl_batsman_score i1 
where i1.score > ALL( select i2.score from ipl_batsman_score i2 
                      where i1.batsman_id  = i2.batsman_id 
                      and i1.match_id  > i2.match_id  )
)where rn <> 1
group by batsman_id;
```
</details>  

## Begin and End Price

```sql
drop table price ;
create table price ( product_id varchar2(50), price number , effective_date date);

insert into price values (1,400,to_Date('01-Jan-2019','DD-MON-YYYY'));
insert into price values (2,300,to_Date('01-Jan-2019','DD-MON-YYYY'));
insert into price values (1,400,to_Date('02-Jan-2019','DD-MON-YYYY'));
insert into price values (2,300,to_Date('02-Jan-2019','DD-MON-YYYY'));
insert into price values (1,500,to_Date('03-Jan-2019','DD-MON-YYYY'));
insert into price values (2,300,to_Date('03-Jan-2019','DD-MON-YYYY'));
insert into price values (1,500,to_Date('04-Jan-2019','DD-MON-YYYY'));
insert into price values (2,300,to_Date('04-Jan-2019','DD-MON-YYYY'));
insert into price values (1,600,to_Date('05-Jan-2019','DD-MON-YYYY'));
insert into price values (2,400,to_Date('05-Jan-2019','DD-MON-YYYY'));
insert into price values (1,600,to_Date('06-Jan-2019','DD-MON-YYYY'));
insert into price values (2,400,to_Date('06-Jan-2019','DD-MON-YYYY'));
```
<details>
<summary>Answer</summary>

```sql
select min(effective_date) begin_dt , max(effective_Date) end_dt , price , product_id  
from(select 
sum(case when lag_price <> price then 1 else 0 end) over(partition by product_id order by effective_Date ) rng , product_id , effective_Date, price
from
(select lag(price) over(partition by product_id order by effective_Date)  lag_price , price , effective_Date, product_id  from price ))
group by price , product_id , rng;
```
</details>

## Consecutive winning streak

```sql
create table MATCH_RESULTS
(
MATCH_ID NUMBER GENERATED ALWAYS AS IDENTITY,
TEAMA VARCHAR2(255),
TEAMB VARCHAR2(255),
WINNER VARCHAR2(255)
);

insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('IND','WI','WI');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('SA','WI','WI');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('IND','AUS','IND');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('WI','ENG','WI');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('PAK','WI','PAK');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('IND','SA','IND');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('SA','ENG','ENG');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('ENG','AUS','AUS');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('AUS','PAK','AUS');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('PAK','WI','WI');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('WI','IND','IND');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('IND','ENG','IND');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('IND','PAK','IND');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('WI','AUS','WI');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('ENG','WI','WI');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('ENG','SA','ENG');
insert into MATCH_RESULTS (TEAMA, TEAMB, WINNER) VALUES('ENG','IND','ENG');
```

<details>
<summary>Answer</summary>

```sql
with temp as
(
select teama, count(res) cnt from
(
select teama, match_id, winner,
sum(case when winner <> prev then 1 else 0 end) over(partition by teama order by match_id) res
from
(
select m2.teama, m1.match_id, m1.winner, 
lag(m1.winner) over (partition by m2.teama order by match_id) prev
from match_results m1
join 
(select distinct teama from match_results
union
select distinct teamb from match_results) m2
on (m2.teama = m1.teama or m2.teama = m1.teamb)
)
)
group by teama, res
order by teama, res
)
select teama, cnt from temp where
cnt = (select max(cnt) from temp); 
```
</details>

## Users were active in the month of January

```sql
create table user_details 
( user_id NUMBER GENERATED ALWAYS AS IDENTITY,
start_dt DATE , 
end_dt DATE );

insert into user_Details(start_dt,end_dt) values ('01-Jul-2019','01-Aug-2019');
insert into user_Details(start_dt,end_dt) values ('01-Jul-2019','01-Jan-2020');
insert into user_Details(start_dt,end_dt) values ('01-Jan-2020','31-Jan-2020');
insert into user_Details(start_dt,end_dt) values ('15-Jan-2020','01-Feb-2020');
insert into user_Details(start_dt,end_dt) values ('01-Feb-2020','10-Feb-2020');
insert into user_Details(start_dt,end_dt) values ('01-Jul-2019','10-Feb-2020');
```
<details>
<summary>Answer</summary>
  
```sql
select * from user_details where end_dt >= '01-Jan-2020' and start_dt <='31-Jan-2020' ; 
```

</details>  

## Worst case and Best case with joins

```sql
DROP TABLE tb1;
DROP TABLE tb2;
CREATE TABLE TB1(
pk NUMBER);

CREATE TABLE TB2(
pk NUMBER);

insert into tb1 values(1);
insert into tb1 values(1);
insert into tb1 values(1);
insert into tb1 values(2);
insert into tb1 values(3);

insert into tb2 values(1);
insert into tb2 values(1);
insert into tb2 values(3);
insert into tb2 values(4);

select * from tb1 inner join tb2 using(pk) ;
select * from tb1 left outer join tb2 using(pk) ;
select * from tb1 right outer join tb2 using(pk) ;
select * from tb1 full outer join tb2 using(pk) ;
select * from tb1 cross join tb2 ;
```

<details>
<summary>Answer</summary>

7, 8, 8, 9, 20

</details>  

## Three consecutive seats availability in theatres
```sql
create table seats(
seat_id NUMBER GENERATED ALWAYS AS IDENTITY,
AVAILABILITY VARCHAR2(1)
);

insert into seats(availability) values('Y');
insert into seats(availability) values('Y');
insert into seats(availability) values('N');
insert into seats(availability) values('Y');
insert into seats(availability) values('N');
insert into seats(availability) values('N');
insert into seats(availability) values('Y');
insert into seats(availability) values('Y');
insert into seats(availability) values('Y');
insert into seats(availability) values('Y');
insert into seats(availability) values('N');
insert into seats(availability) values('N');
insert into seats(availability) values('Y');
insert into seats(availability) values('Y');
insert into seats(availability) values('Y');
insert into seats(availability) values('N');
```

<details>
<summary>Answer</summary>
  
```sql
select * from seats s1 where  s1.availability ='Y' 
and 'Y'= ALL(select s2.availability from seats s2 where s2.seat_id between s1.seat_id and s1.seat_id + 2 ) 
order by 1 ;
```
</details>  

## Create all combinations of teams from one table

```sql
create table teams(
team varchar2(10)
);

insert into teams values('IND');
insert into teams values('SA');
insert into teams values('ENG');
insert into teams values('AUS');
```

<details>
<summary>Answer</summary>

```sql  
select t1.team, t2.team from teams t1 cross join teams t2 where t1.team < t2.team ;
```

</details>

## Frequently brought products in a table
```sql
create table order_line
(
order_id number ,
order_line_id number,
product varchar2(1)
);

insert into order_line values(1, 1, 'A');
insert into order_line values(1, 2, 'B');
insert into order_line values(1, 3, 'C');
insert into order_line values(1, 4, 'D');

insert into order_line values(2, 1, 'B');
insert into order_line values(2, 2, 'C');

insert into order_line values(3, 1, 'D');
insert into order_line values(3, 2, 'A');

insert into order_line values(4, 1, 'B');
insert into order_line values(4, 2, 'A');

insert into order_line values(5, 1, 'C');
insert into order_line values(5, 2, 'A');

insert into order_line values(6, 1, 'C');
insert into order_line values(6, 2, 'A');
insert into order_line values(6, 3, 'B');

insert into order_line values(7, 1, 'A');
insert into order_line values(7, 2, 'C');
insert into order_line values(7, 3, 'D');
```

<details>
<summary>Answer</summary>
  
```sql
select o1.product, o2.product , count(*)
from order_line o1 join order_line o2
on o1.order_id = o2.order_id
and o1.product <> o2.product
where o1.product < o2.product
group by o1.product, o2.product
order by 3 desc;
```

</details>  


## Calculate how many batsman have scored more than 15% runs as compared to last year
```sql
create table ipl_score
(batsman_id number , match_id number, score number, year_date number);

insert all
into ipl_score values (1,1,10,2018)
into ipl_score values (1,2,15,2018)
into ipl_score values (1,3,100,2018)
into ipl_score values (2,1,101,2018)
into ipl_score values (2,2,115,2018)
into ipl_score values (2,3,90,2018)
into ipl_score values (1,1,45,2019)
into ipl_score values (1,2,155,2019)
into ipl_score values (1,3,60,2019)
into ipl_score values (2,1,10,2019)
into ipl_score values (2,2,20,2019)
into ipl_score values (2,3,100,2019)
select 1 from dual;
```
<details>
<summary>Answer</summary>
  
```sql
select 
batsmen_id,
year
from(
select 
batsmen_id, 
year, 
(lag(sum_run) over(partition by batsmen_id, year) - sum_run)/sum_run *100 inc_pct
(
select sum(runs) sum_run , batsmen_id, year  from runs 
group by batsmen_id, year )) where inc_pct > 15;
```

</details>  

## Check which department has more combined salary than 500000

```sql
create table emp
(empid number,
deptid number,
salary number
);

insert into emp values(1, 1, 100000);
insert into emp values(2, 1, 200000);
insert into emp values(3, 1, 300000);
insert into emp values(4, 2, 500000);
insert into emp values(5, 3, 400000);
insert into emp values(6, 3, 100000);
insert into emp values(7, 4, 800000);
insert into emp values(8, 5, 200000);
```

<details>
<summary>Answer</summary>
  
```sql
select d.department_id from 
employees e 
join departments d 
on e.department_id=d.department_id
group by d.department
having sum(e.salary) > 500000;
```

```sql
select deptid, sum(salary)
from emp
group by deptid
having sum(salary) > 500000;
```
</details>

## Compare current week’s data to previous year’s corresponding week’s data

<details>
<summary>Answer</summary>
  
```sql
select ROUND((to_date('20-APR-2020','DD-MON-YYYY') - to_date('01-JAN-2020','DD-MON-YYYY'))/7)+1 from dual;
```
</details>

## Last 3 orders of a customer
```sql
create table orders(
order_id number,
customer_id number,
order_date date,
order_time varchar(20),
order_amount number);

create table order_line(
order_id number,
order_line_id number,
product_id number,
delivery_address_id number);

create table deliver_address(
delivery_address_id number,
address_line_1 varchar2(50),
address_line_2 varchar2(50),
address_line_3 varchar2(50),
pin number,
state varchar2(50));

insert into orders values(1, 1, to_date('08-APR-2020','DD-MON-YYYY'),'10AM',500);
insert into orders values(2, 1, to_date('09-APR-2020','DD-MON-YYYY'),'10AM',700);
insert into orders values(3, 2, to_date('08-APR-2020','DD-MON-YYYY'),'9AM',400);
insert into orders values(4, 2, to_date('10-APR-2020','DD-MON-YYYY'),'10AM',500);
insert into orders values(5, 3, to_date('08-APR-2020','DD-MON-YYYY'),'10AM',800);

insert into order_line values(1,1,104,1);
insert into order_line values(1,2,107,2);
insert into order_line values(2,1,103,3);
insert into order_line values(2,2,105,3);
insert into order_line values(2,3,107,3);
insert into order_line values(3,1,102,4);
insert into order_line values(4,1,110,4);
insert into order_line values(5,1,100,5);
insert into order_line values(5,2,101,5);

insert into deliver_address values(1,'lajpat','nagar',null,110010,'Delhi');
insert into deliver_address values(2,'402 house','sector52','dwarka',110201,'Delhi');
insert into deliver_address values(3,'pratap','nagar',null,110310,'Delhi');
insert into deliver_address values(4,'palm greens','sector90','aerospace',110402,'Gurgaon');
insert into deliver_address values(5,'50garden','greens','southex',110120,'Delhi');
```
<details>
<summary>Answer</summary>

```sql
select * from 
(select  o.*, da.delivery_address_id,
dense_rank() over (order by o.order_id desc) rn
from orders o join order_line ol on o.order_id = ol.order_id
join deliver_address da on ol.delivery_address_id = da.delivery_address_id
where o.customer_id = 1 
) where rn <=3;
```

</details>

## Top five views per year and increase consistent views per year
```sql
create table song_dim (
song_id number,
song_name varchar2(255));

create table views_fact(
song_id number,
views number,
eff_dt date)

create table date_dim(
eff_dt date,
eff_year number
);

insert into song_dim values(1, 'coldplay');
insert into song_dim values(2, 'somebody');
insert into song_dim values(3, 'numb');
insert into song_dim values(4, 'our song');
insert into song_dim values(5, 'complicated');
insert into song_dim values(6, 'dance');
insert into song_dim values(7, 'shape');


insert into views_fact values(1, 10, to_date('08-APR-2020','DD-MON-YYYY'));
insert into views_fact values(2, 15, to_date('08-APR-2020','DD-MON-YYYY'));
insert into views_fact values(1, 10, to_date('09-APR-2020','DD-MON-YYYY'));
insert into views_fact values(2, 25, to_date('09-APR-2020','DD-MON-YYYY'));
insert into views_fact values(3, 40, to_date('10-APR-2020','DD-MON-YYYY'));
insert into views_fact values(2, 10, to_date('06-APR-2020','DD-MON-YYYY'));
insert into views_fact values(3, 20, to_date('16-APR-2020','DD-MON-YYYY'));
insert into views_fact values(4, 70, to_date('10-APR-2020','DD-MON-YYYY'));
insert into views_fact values(5, 90, to_date('10-APR-2020','DD-MON-YYYY'));
insert into views_fact values(6, 10, to_date('09-APR-2020','DD-MON-YYYY'));
insert into views_fact values(7, 15, to_date('09-APR-2020','DD-MON-YYYY'));
insert into views_fact values(1, 50, to_date('16-AUG-2019','DD-MON-YYYY'));
insert into views_fact values(2, 70, to_date('08-AUG-2019','DD-MON-YYYY'));
insert into views_fact values(3, 80, to_date('10-AUG-2019','DD-MON-YYYY'));
insert into views_fact values(6, 20, to_date('09-AUG-2019','DD-MON-YYYY'));
insert into views_fact values(7, 20, to_date('09-AUG-2019','DD-MON-YYYY'));
insert into views_fact values(1, 20, to_date('06-APR-2020','DD-MON-YYYY'));
insert into views_fact values(2, 30, to_date('05-APR-2020','DD-MON-YYYY'));
insert into views_fact values(3, 50, to_date('07-APR-2020','DD-MON-YYYY'));


insert into date_dim values(to_date('06-APR-2020','DD-MON-YYYY'), 2020);
insert into date_dim values(to_date('07-APR-2020','DD-MON-YYYY'), 2020);
insert into date_dim values(to_date('08-APR-2020','DD-MON-YYYY'), 2020);
insert into date_dim values(to_date('09-APR-2020','DD-MON-YYYY'), 2020);
insert into date_dim values(to_date('10-APR-2020','DD-MON-YYYY'), 2020);
insert into date_dim values(to_date('16-APR-2020','DD-MON-YYYY'), 2020);
insert into date_dim values(to_date('08-AUG-2019','DD-MON-YYYY'), 2019);
insert into date_dim values(to_date('09-AUG-2019','DD-MON-YYYY'), 2019);
insert into date_dim values(to_date('10-AUG-2019','DD-MON-YYYY'), 2019);
insert into date_dim values(to_date('16-AUG-2019','DD-MON-YYYY'), 2019);
```

<details>
<summary>Answer</summary>

```sql
select * from
(
select sv.*, dense_rank() over (order by sv.sum_views desc) rn
from
(
select vf.song_id, sum(vf.views) sum_views
from views_fact vf join date_dim dd on vf.eff_dt = dd.eff_dt
and dd.eff_year = '2020'
group by vf.song_id
)sv
) where rn <=5;

select * from
(
select sv.*,
sum_views  - lag(sum_views) over (partition by song_id order by eff_year)  diff
from
(select vf.song_id, dd.eff_year, sum(vf.views) sum_views 
from views_fact vf join date_dim dd on vf.eff_dt = dd.eff_dt
group by vf.song_id, dd.eff_year
order by vf.song_id, dd.eff_year
) sv
) where diff > 0;
```

</details>

## Pivot without using pivot function
```sql

create table emp
(empid number,
deptid number,
salary number
);

insert into emp values(1, 1, 100000);
insert into emp values(2, 1, 200000);
insert into emp values(3, 1, 300000);
insert into emp values(4, 2, 500000);
insert into emp values(5, 3, 400000);
insert into emp values(6, 3, 100000);
insert into emp values(7, 4, 800000);
insert into emp values(8, 5, 200000);
```

<details>
<summary>Answer</summary>

```sql
with temp as
(
select
empid,
sum(case when salary between 100000 and 200000 then 1 end) as sal1,
sum(case when salary between 200001 and 500000 then 1 end) as sal2,
sum(case when salary > 500000  then 1 end) as sal3
from emp
group by empid
)
select 'TYPE' as cnt, sum(sal1), sum(sal2), sum(sal3)
from temp;

with t as (
select 
sum(case when d.deptno=30 then e.sal end) sum_30, 
sum(case when d.deptno=10 then e.sal end) sum_10,
sum(case when d.deptno=20 then e.sal end) sum_20
from 
scott.emp e join 
scott.dept d 
on e.deptno=d.deptno 
group by d.deptno )
select max(sum_30) sum_30, max(sum_10) sum_10, max(sum_20) sum_20 from t;
```
</details>

## Apply lag function without using lag function

Order_day| Processing_Day | Quantity| Order_id

```sql
create table 
orders (
Order_day date ,  Processing_Day date ,  Quantity NUMBER , Order_id NUMBER);

insert into orders values('12-FEB-2020','13-FEB-2020', 2, 1); 
insert into orders values('12-FEB-2020','15-FEB-2020', 4, 1);
insert into orders values('12-FEB-2020','16-FEB-2020', 5, 1);
insert into orders values('12-FEB-2020','18-FEB-2020', 3, 1);
insert into orders values('11-FEB-2020','19-FEB-2020', 2, 3);
insert into orders values('11-FEB-2020','22-FEB-2020', 10, 3);
insert into orders values('11-FEB-2020','21-FEB-2020', 8, 3);
insert into orders values('11-FEB-2020','27-FEB-2020', 17, 3);
```

Processing_Day | Order_day | Order_id | change_in_quantity

<details>
<summary>Answer</summary>

```sql
select 
* 
from
orders o1 
left outer join 
orders o2 
on o1.order_day=o2.order_Day and o1.order_id=o2.order_id
and o2.processing_day = (select max(o3.processing_Day) from  orders o3 where o1.order_day=o3.order_day and o1.order_id=o3.order_id and o3.processing_day < o1.processing_Day); 

select o1.order_day, o1.order_id, o1.processing_day, max(o2.processing_day)
from orders o1 left outer join orders o2 on o1.order_id = o1.order_id
and o1.order_day = o2.order_day
and o1.processing_day > o2.processing_day
group by o1.order_day, o1.order_id, o1.processing_day
order by 1, 2
```

#### Generic Query for lag without lag function

``` sql
create table tab1(col1 number);

insert into tab1 values (1);
insert into tab1 values (2);
insert into tab1 values (3);
insert into tab1 values (4);
insert into tab1 values (5);
insert into tab1 values (7);


select * from tab1 t1 left outer join tab1 t2 on t1.col1 > t2.col1
and t2.col1 = (select max(t3.col1) from tab1 t3 where t3.col1 < t1.col1)

select t1.col1, max(t2.col1)
from tab1 t1 left outer join tab1 t2 on t1.col1 > t2.col1
group by t1.col1
order by t1.col1;
```

</details>

## Count how many employee id are under a manager table. 

Empid | Manager_id

```sql
create table emp
( empid number,
  ename varchar2(50),
  deptid number,
  mgrid number
);

insert into emp values(1,'John',1,null);
insert into emp values(2,'Mary',1,1);
insert into emp values(3,'Rose',1,1);
insert into emp values(4,'Mac',2,null);
insert into emp values(5,'Paul',2,4);
insert into emp values(6,'Elsa',2,5);
insert into emp values(7,'Belly',2,5);
```

<details>
<summary>Answer</summary>

```sql
select e1.ename  from scott.emp e1 
left outer join 
(select e.mgr emp from scott.emp e group by mgr having count(*) > 3) e2
on e1.empno=e2.emp;
```
</details>

## Total value for a seller on a day

| Seller_id  | Start_Date | end_Date | Seller_name |
Order_id | Order_day | Seller_id | Price|Quantity

```sql
create table seller
(seller_id number,
seller_name varchar2(20),
start_date date,
end_date date);

create table orders
(
order_id number,
order_day date,
seller_id number,
price number,
quantity number);

insert into seller values(1,'Paul','01-APR-2020',null);
insert into seller values(2,'John','01-JAN-2020','31-MAR-2020');
insert into seller values(3,'Mic','01-FEB-2020',null);
insert into seller values(4,'Mary','10-MAR-2020',null);

insert into orders values(1,'20-FEB-2020',2,100,3);
insert into orders values(2,'11-JAN-2020',2,50,6);
insert into orders values(3,'25-APR-2020',1,200,1);
insert into orders values(4,'20-APR-2020',2,75,2);
insert into orders values(5,'30-MAR-2020',3,300,2);
insert into orders values(6,'20-APR-2020',4,10,5);
insert into orders values(7,'20-FEB-2020',3,500,1);
```

<details>
<summary>Answer</summary>

```sql
select 
s.seller_id , sum(price*quantity) 
from 
seller s 
left outer join 
order o
on s.seller_id=o.seller_id and o.order_day between s.start_Date and NVL(s.end_Date,'31-DEC-9999')
group by s.seller_id ;
```

</details>

## Movie Reviewer Query on DWH
```sql
create table movie 
(
movie_id number,
movie_name varchar2(255)
);

create table reviewer
(
reviewer_id number,
reviewer_name varchar2(255)
);

create table rating
(
rating_id number,
movie_id number,
reviewer_id number,
rating_date date,
ratings number
);

insert into movie values (1,'A');
insert into movie values (2,'B');
insert into movie values (3,'C');
insert into movie values (4,'D');
insert into movie values (5,'E');

insert into reviewer values (1,'aa');
insert into reviewer values (2,'bb');
insert into reviewer values (3,'cc');
insert into reviewer values (4,'dd');
insert into reviewer values (5,'ee');
insert into reviewer values (6,'ff');

insert into rating values(1,1,2,to_date('20170201','YYYYMMDD'),3.1);
insert into rating values(2,4,4,to_date('20180301','YYYYMMDD'),4.2);
insert into rating values(3,1,5,to_date('20190401','YYYYMMDD'),5);
insert into rating values(4,3,1,to_date('20170501','YYYYMMDD'),3.3);
insert into rating values(5,6,3,to_date('20180601','YYYYMMDD'),4.4);
insert into rating values(6,2,2,to_date('20190701','YYYYMMDD'),5);
insert into rating values(7,5,1,to_date('20200801','YYYYMMDD'),3.6);
insert into rating values(8,2,3,to_date('20170901','YYYYMMDD'),4.5);
insert into rating values(9,1,4,to_date('20181001','YYYYMMDD'),5);
insert into rating values(10,5,1,to_date('20170201','YYYYMMDD'),3.7);
insert into rating values(11,2,5,to_date('20170201','YYYYMMDD'),4.5);
insert into rating values(12,3,6,to_date('20170201','YYYYMMDD'),3.5);
insert into rating values(13,2,3,to_date('20180201','YYYYMMDD'),4.7);
insert into rating values(14,1,4,to_date('20180301','YYYYMMDD'),4);
insert into rating values(15,5,1,to_date('20180401','YYYYMMDD'),3.8);
insert into rating values(16,2,5,to_date('20180501','YYYYMMDD'),3.9);
insert into rating values(17,3,6,to_date('20180601','YYYYMMDD'),4.5);
```

#### Those who reviewed movie better than first time(with and without analytical functions)

<details>
<summary>Answer</summary>

```sql
select re.reviewer_name from (
select 
reviewer_id , 
ratings,
lag(ratings) over(partition by movie_id, reviewer_id order by rating_date ) lag_rating
from rating 
) r 
left outer join reviewer re on  r.reviewer_id=re.reviewer_id
where ratings > lag_rating ;

select 
re.reviewer_name
from 
reviewer re
left outer join (
select 
ra1.*,
ra2.ratings lag_ratings
from 
rating ra1 
left outer join 
rating ra2
on ra1.movie_id=ra2.movie_id and ra1.reviewer_id=ra2.reviewer_id 
where ra2.rating_date = (select max(rating_date) from rating ra3 where ra3.reviewer_id=ra1.reviewer_id and ra3.movie_id=ra1.movie_id and ra3.rating_date < ra1.rating_date 
)) r 
on re.reviewer_id = r.reviewer_id 
where ratings > lag_ratings  ;

select rw.reviewer_name from reviewer rw join
(
select r1.movie_id, r1.reviewer_id, r1.rating_date, max(r2.rating_date), (min(r1.ratings) - min(r2.ratings)) diff
from rating r1 left outer join rating r2 
on r1.movie_id = r2.movie_id  
and r1.reviewer_id =  r2.reviewer_id
and r1.rating_date > r2.rating_date
--order by 2,3,4,5,7,8,9
group by r1.movie_id, r1.reviewer_id, r1.rating_date
having (min(r1.ratings) - min(r2.ratings)) > 0
order by 1,2,3
) temp on rw.reviewer_id = temp.reviewer_id

```
</details>

#### Create a report with reviewername|2017|2018|2019

<details>
<summary>Answer</summary>
  
```sql
select 
re.reviewer_name , 
sum_rating."2017",
sum_rating."2018",
sum_rating."2019"
from 
reviewer re 
left outer join
(
select 
sum(case when to_char(rating_date,'YYYY') = '2017' then 1 else 0 end) as "2017" ,
sum(case when to_char(rating_date,'YYYY') = '2018' then 1 else 0 end) as "2018" ,
sum(case when to_char(rating_date,'YYYY') = '2019' then 1 else 0 end) as "2019" ,
reviewer_id 
from
rating ra 
group by reviewer_id) sum_rating
on sum_rating.reviewer_id = re.reviewer_id
;
```
</details>

### Reviewer name whose have not given rating date.

<details>
<summary>Answer</summary>

```sql
select 
distinct r.reviewer_name
from 
reviewer r 
left outer join
rating ra
on r.reviewer_id=ra.reviewer_id
where rating_date is null;
```

</details>

## Print dept and emp with max salary(analytical fns , group by IN clause, joins)
```sql
create table emp
(empid number,
deptid number,
salary number
);

insert into emp values(1, 1, 100000);
insert into emp values(2, 1, 200000);
insert into emp values(3, 1, 300000);
insert into emp values(4, 2, 500000);
insert into emp values(5, 3, 400000);
insert into emp values(6, 3, 100000);
insert into emp values(7, 4, 800000);
insert into emp values(8, 5, 200000);
```

<details>
<summary>Answer</summary>

```sql
select 
e1.* 
from scott.emp e1 
join (
select 
max(sal) max_sal,
deptno
from
scott.emp 
group by deptno ) e2 
on e1.sal=e2.max_sal and e1.deptno=e2.deptno;

select * from emp
where (deptid, salary) in (
select deptid, max(salary) max_sal
from emp
group by deptid
);

select e.* 
from emp e join
(
select deptid, max(salary) max_sal
from emp
group by deptid) max_emp
on e.deptid = max_emp.deptid
and e.salary = max_emp.max_sal;

select * from
(select e.*, dense_rank() over (partition by deptid order by salary desc) rn
from emp e
) where rn = 1;

```
</details>

## Find batsman who scored 3 consecutive dots

Innid|Overid|Ball_num|Batsman|Bowler|Runs_scored|Is_wide|Is_noball|Is_freehit

<details>
<summary>Answer</summary>

```sql

select 
sum(case when lag_run_Scored <> runs_scored then 1 else 0 end)
over(partition by innid,batsman order by over_id,ball_num) lag_run_scored, 
batsman,
runs_scored
from(
select 
lag(runs_scored) over(partition by innid,batsman order by over_id,ball_num) lag_run_scored, 
batsman,
runs_scored
from match_details md
where is_wide='N' and is_noball='N' ) 

```

</details>
  
## All combination with NULL and joins

```sql
CREATE TABLE TB1(
pk NUMBER);

CREATE TABLE TB2(
pk NUMBER);

insert into tb1 values(1);
insert into tb1 values(1);
insert into tb1 values(1);
insert into tb1 values(2);
insert into tb1 values(3);
insert into tb1 values(null);

insert into tb2 values(1);
insert into tb2 values(1);
insert into tb2 values(3);
insert into tb2 values(4);
insert into tb2 values(null);

select * from tb1 inner join tb2 using(pk) ;
select * from tb1 left outer join tb2 using(pk) ;
select * from tb1 right outer join tb2 using(pk) ;
select * from tb1 full outer join tb2 using(pk) ;
select * from tb1 cross join tb2 ;
```

<details><summary>Answer</summary>

7,9,9,11,30
  
</details>  

## Ticket table , Audit Table Id
TID|Impact|CreateTime|
AID|TID|CreateTime|Team

#### Tickets count Group per week basis on Team, Impact

```sql
create table ticket
(
tid number,
impact varchar2(10),
createtime date
);

create table audits
(
aid number,
tid number,
createtime date,
team varchar2(10)
);

insert into ticket values(1, 'sev1', '10-APR-2020');
insert into ticket values(2, 'sev2', '03-APR-2020');
insert into ticket values(3, 'sev1', '28-MAR-2020');
insert into ticket values(4, 'sev2', '21-MAR-2020');
insert into ticket values(5, 'sev3', '11-MAR-2020');


insert into audits values (1, 1, '10-APR-2020', 'A');
insert into audits values (2, 2, '03-APR-2020', 'B');
insert into audits values (3, 1, '10-APR-2020', 'A');
insert into audits values (4, 2, '03-APR-2020', 'B');
insert into audits values (5, 3, '28-MAR-2020', 'A');
insert into audits values (6, 4, '28-MAR-2020', 'C');
insert into audits values (7, 4, '21-MAR-2020', 'A');
insert into audits values (8, 5, '11-MAR-2020', 'B');
```

<details>
<summary>Answer</summary>
  
```sql
select
to_week(createtime) , team,impact,count(*)
from(
select 
distinct t.* , a.team
from 
ticket t
join
audit a 
on t.tid=a.tid)
group by to_week(createtime) , team,impact

select t.createtime, a.team, t.impact, count(*)
from ticket t join audits a on t.tid = a.tid
group by a.team, t.impact, t.createtime;
```

</details>

## Select all customer id and its latest version
```sql
create table customer_hist(
customer_id number,
customer_prev_id number,
customer_succ_id number);

insert into customer_hist values(100,95,102);
insert into customer_hist values(101,96,105);
insert into customer_hist values(102,100,104);
insert into customer_hist values(103,99,106);
insert into customer_hist values(107,88,111);
```

<details>
<summary>Answer</summary>

```sql
with tab1 as (
select customer_prev_id cust_id , customer_id cust_prev_id from customer_hist
union
select customer_id cust_id , customer_succ_id cust_prev_id from customer_hist)
, t1 as (select cust_id, cust_prev_id  , level lvl , CONNECT_BY_ROOT cust_prev_id cust_latest_id
from tab1
CONNECT BY PRIOR cust_id = cust_prev_id )
select cust_id, cust_latest_id from(
select cust_id, cust_latest_id , dense_rank() over(partition by cust_id order by lvl desc) rn , lvl from t1)
where rn =1 order by 1;
```
</details>

## Difference in salary with current employee and minimum salary greater than his/her

<details>
<summary>Answer</summary>
  
```sql  
select employeeid , salary, lead(salary) over(order by salary) - salary as diff_salary   from Employee_Yearly_Salary;
```

</details>

## Printing all combinations with employees

<details>
  <summary>Answer</summary>
  
```sql
with rec_table(empno,mgr,path) as
(
select empno , null as mgr , to_char(empno) path from scott.emp where mgr is null
union all
select e.empno , e.mgr , r.path || ',' || e.empno path from rec_table r join scott.emp e on e.mgr=r.empno )
select * from rec_table ;
```
</details>

## Find all products sold in november 2018
```sql
create table orders(
order_id number,
customer_id number,
order_date date,
order_time varchar(20),
order_amount number);

create table order_line(
order_id number,
order_line_id number,
product_id number,
delivery_address_id number);

create table deliver_address(
delivery_address_id number,
address_line_1 varchar2(50),
address_line_2 varchar2(50),
address_line_3 varchar2(50),
pin number,
state varchar2(50));

create table date_dim (
eff_dt date,
eff_year number
);

create table product (
pid number,
pname varchar2(20)
);

insert into orders values(1, 1, to_date('08-APR-2020','DD-MON-YYYY'),'10AM',500);
insert into orders values(2, 2, to_date('09-APR-2020','DD-MON-YYYY'),'10AM',700);
insert into orders values(3, 1, to_date('08-APR-2019','DD-MON-YYYY'),'9AM',400);
insert into orders values(4, 2, to_date('10-APR-2019','DD-MON-YYYY'),'10AM',500);
insert into orders values(5, 1, to_date('08-NOV-2018','DD-MON-YYYY'),'10AM',800);
insert into orders values(6, 2, to_date('08-NOV-2018','DD-MON-YYYY'),'8AM',700);
insert into orders values(7, 3, to_date('09-NOV-2018','DD-MON-YYYY'),'9AM',400);
insert into orders values(8, 1, to_date('10-NOV-2018','DD-MON-YYYY'),'10AM',500);

insert into order_line values(1,1,104,1);
insert into order_line values(2,2,107,2);
insert into order_line values(3,1,103,3);
insert into order_line values(4,2,105,3);
insert into order_line values(5,3,107,3);
insert into order_line values(1,1,102,4);
insert into order_line values(2,1,110,4);
insert into order_line values(3,1,100,5);
insert into order_line values(4,2,101,5);
insert into order_line values(6,2,111,1);
insert into order_line values(7,1,201,1);
insert into order_line values(8,1,311,2);
insert into order_line values(7,3,104,2);

insert into deliver_address values(1,'lajpat','nagar',null,110010,'Delhi');
insert into deliver_address values(2,'402 house','sector52','dwarka',110201,'Delhi');
insert into deliver_address values(3,'pratap','nagar',null,110310,'Delhi');
insert into deliver_address values(4,'palm greens','sector90','aerospace',110402,'Gurgaon');
insert into deliver_address values(5,'50garden','greens','southex',110120,'Delhi');

insert into date_dim values (to_date('08-NOV-2018','DD-MON-YYYY'), 2018);
insert into date_dim values (to_date('09-NOV-2018','DD-MON-YYYY'), 2018);
insert into date_dim values (to_date('10-NOV-2018','DD-MON-YYYY'), 2018);
insert into date_dim values (to_date('08-APR-2019','DD-MON-YYYY'), 2019);
insert into date_dim values (to_date('10-APR-2019','DD-MON-YYYY'), 2019);
insert into date_dim values (to_date('08-APR-2019','DD-MON-YYYY'), 2020);
insert into date_dim values (to_date('09-APR-2019','DD-MON-YYYY'), 2020);

insert into product values (100,'talc');
insert into product values (101,'cream');
insert into product values (102,'spices');
insert into product values (103,'water');
insert into product values (104,'milk');
insert into product values (105,'egg');
insert into product values (107,'bread');
insert into product values (108,'banana');
insert into product values (109,'apple');
insert into product values (110,'coke');
insert into product values (111,'pepsi');
insert into product values (201,'orange');
insert into product values (311,'oil');
```

<details>
  <summary>Answer</summary>

```sql
select 
p.product_name
from
(select 
distinct ol.product_id  
from orders o
left outer join 
order_line on o.order_id=ol.order_id
left outer join date d on d.day=o.order_day
where d.month='November' and d.year=2018
) tbl
inner join products p 
on p.product_id=tbl.product_id

select distinct ol.product_id, p.pname
from orders o join order_line ol on o.order_id = ol.order_id
join date_dim dd on dd.eff_dt = o.order_date and dd.eff_year = 2018 
join product p on ol.product_id = p.pid
```

</details>

## Sales rep who sold most amount of products as per amount 
```sql
create table sales
(salesid number,
name varchar2(50)
);

create table orders
( orderid number,
  salesid number,
  productid number,
  amount number
);

insert into sales values(1, 'John');
insert into sales values(2, 'Mary');
insert into sales values(3, 'Rosy');
insert into sales values(4, 'Paul');

insert into orders values (1, 1, 101, 350);
insert into orders values (2, 2, 111, 200);
insert into orders values (3, 1, 201, 700);
insert into orders values (4, 2, 107, 400);
insert into orders values (5, 3, 105, 300);
insert into orders values (6, 4, 101, 350);
```

<details>
<summary>Answer</summary>

```sql
select 
r.repr_name , sum(ol.amt)
from
orders o 
left join 
order_line ol
on o.order_id=ol.order_id
left join 
representative r 
on r.repr_id=o.repr_id 
group by r.repr_name 

with temp as(
select s.salesid, sum(amount) amt
from sales s join orders o on s.salesid = o.salesid
group by s.salesid
)select s.*, temp.amt from temp join sales s on s.salesid = temp.salesid
where amt = (select max(amt) from temp)
```
</details>

## Each student with maximum marks and students
```sql
CREATE TABLE STUDENTS
(
student_id NUMBER ,
subject VARCHAR2(100),
marks NUMBER
);

insert into students values(1,'ENG',90);
insert into students values(1,'HIN',93);
insert into students values(1,'MATHS',92);
insert into students values(11,'ENG',97);
insert into students values(11,'HIN',93);
insert into students values(11,'MATHS',92);
```

<details>
<summary>Answer</summary>
  
```sql
select * from students where (student_id,marks) IN (
select student_id , max(marks) from students group by student_id );

select s1.* from students s1 inner join  (
select student_id , max(marks) as marks from students group by student_id ) s2
on s1.student_id=s2.student_id and s1.marks=s2.marks;

select student_id,marks,subject from
(select 
s1.student_id, s1.marks,s1.subject , dense_rank() over(partition by  student_id order by marks desc) rn 
from 
students s1) t
where rn=1;
```

</details>

## Running sum of customer data

```sql

CREATE TABLE customers 
(
customer_id NUMBER , 
month_id NUMBER ,
amount NUMBER
);

insert into customers values (1,1,100);
insert into customers values (1,2,200);
insert into customers values (1,4,150);
insert into customers values (2,5,100);
insert into customers values (2,12,200);
insert into customers values (2,7,200);
```

<details>
<summary>Answer</summary>

```sql
select c.*,
sum(amount) over(partition by customer_id order by month_id) running_sum
from customers c 
  
select c2.customer_id, c2.month_id, sum(c1.amount)
from customers c1 join customers c2
on c1.customer_id = c2.customer_id
and c1.month_id <= c2.month_id
group by c2.customer_id, c2.month_id
order by c2.customer_id, c2.month_id
```

</details>

## SQL Validation and Tuning

Given four tables ORDERS, REGIONS, ITEMS, DE_NOM_TABLE <De-normalised table containing previous 
day data of first three tables> and a sample SQL query to extract certain information from 
all these tables. Estimated number of rows in each of these tables are 20 million, 6000, 
50 million and 30 million respectively. All the columns in above mentioned tables are Nullable. 
Validate whether the SQL query is fine tuned for performance and optimise the query if required.

```sql
 
> ORDERS
order_id	order_date	district
O1			2020/01/09	D1
O2			2020/01/03	D2
O3			2020/01/12	D2
O4			2020/04/09	D3
O5			2020/02/09	D4
O6			2020/02/15	D5
O7			2020/05/12	D6

> ITEMS
order_id	item_id
O1			I3
O1			I1
O1			I2
O2			I4
O2			I5
O3			I6
O4			I7
O5			I8
O6			I11
O6			I10
O6			I9
O7			I12


> REGIONS
city	district	country
C1		D1			R1
C2		D1			R1
C5		D2			R1
C3		D2			R1
C6		D2			R1
C4		D2			R1
C7		D2			R1
C8		D3			R2
C9		D4			R2
C10		D5			R3

=== QUERY ===
SELECT ORDER_ID, ITEM_ID, ORDER_DATE 
FROM DE_NOM_TABLE 
MINUS 
(SELECT T1.ORDER_ID, T2.ITEM_ID, T3.COUNTRY, T1.ORDER_DATE
FROM ORDERS T1
INNER JOIN 
ITEMS T2 ON (T1.ORDER_ID = T2.ORDER_ID)
INNER JOIN 
REGIONS T3 ON (T1.DISTRICT = T3.DISTRICT)
GROUP BY 1,2,3,4
HAVING T1.ORDER_DATE BETWEEN TO_DATE('2020-01-01','YYYY-MM-DD') AND TO_DATE('2020-03-01','YYYY-MM-DD'));
```

<details>
<summary>Answer1</summary>

```sql

SELECT ORDER_ID, ITEM_ID, COUNTRY, ORDER_DATE 
FROM DE_NOM_TABLE T LEFT OUTER JOIN 
(SELECT /*+ PARALLEL */
T1.ORDER_ID, T2.ITEM_ID, T3.COUNTRY, T1.ORDER_DATE
FROM ORDERS T1
INNER JOIN 
ITEMS T2 ON (T1.ORDER_ID = T2.ORDER_ID)
INNER JOIN 
REGIONS T3 ON (T1.DISTRICT = T3.DISTRICT) and T3.country ='R1'
WHERE T1.ORDER_DATE BETWEEN TO_DATE('2020-01-01','YYYY-MM-DD') AND TO_DATE('2020-03-01','YYYY-MM-DD')
GROUP BY T1.ORDER_ID, T2.ITEM_ID, T3.COUNTRY, T1.ORDER_DATE) O
ON T.ORDER_ID = O.ORDER_ID AND T.ITEM_ID = O.ITEM_ID 
AND T.COUNTRY = O.COUNTRY AND T.ORDER_DATE = O.ORDER_DATE
WHERE O.ORDER_ID IS NULL;

```  
</details>  

<details>
<summary>Answer2</summary>
  
```sql

SELECT DISTINCT R.COUNTRY
FROM ORDERS O INNER JOIN REGIONS R ON O.DISTRICT = R.DISTRICT;

SELECT * 
FROM ORDERS O INNER JOIN 
(SELECT DISTINCT R.DISTRICT, R.COUNTRY
FROM REGIONS R
) ON O.DISTRICT = R.DISTRICT;

```
</details>  

## Merge New data to view

I have a view consisting of shipments information. 
The view consists of 1 billion records. The view gets refreshed each day with new or updated data. 
State how will you persist this huge data in the view. 
Write a strategy of how you can effectively merge new data to this view.

Sample Columns:
SHIPMENT_ID    SHIP_DAY

Consider any master table / datastet and create a view on top- of that if needed.

<details>
<summary>Answer</summary>

```sql

CREATE TABLE SHIPMENT
(SHIPMENT_ID NUMBER,
SHIP_DAY     DATE
)
PARTITION ON(SHIP_DAY)
NUMDSTOINTERVAL;

CREATE INDEX 

1. CREATE MATERIALISED VIEW LOG ON SHIPMENT;
CREATE MATERIALISED VIEW SHPMENT_MV
BUILD IMMEDIATE
REFRESH ON COMMIT
AS
SELECT SHIPMENT_ID, SHIP_DAY FROM SHIPMENT;

2. PARTITION TABLE with DAY/MONTH as required.
Whenever data volume is high, PARTITIONING IS MUST.
Use JOIN and FILTERS to effectively fetch the records required ONLY to be UPDATED.
For INSERTION - create new parition with new records.

```
</details>

## Latest Update of an Order

I have a custom data store in which data is stored in files. Each file indicates a partition.
I can only append data to a partition but not delete it. I have a table which consists of Orders data.
Partition of this table is on Order_day. For certain orders order_day keep on changing which means a particular order would 
reside on multiple partitions. Now if I query these partitions I will get multiple entries for a given order_id.
Can you suggest me a strategy so that I get only the lastest update of an order after querying the partitions.

<details>
<summary>Answer</summary>

```sql

order_id  order_date processing_day
1          22/04      22/04
1          23/04      23/04
1          21/04      24/04 


select * from
(
select o.*,
dense_rank() over (order by processing_day desc) rn 
from orders o
where order_id = 1
) where rn = 1

```
</details>  

## Get OrderId and Cost in local currency as on order_date placed

```sql
Orders
order_id customer_id product_id order_date country_code cost(USD)

Customers
customer_id customer_name customer_address email phone_no

Products
product_id product_name category country_code

Exchange_Rate
country_code country rate valid_from valid_to

```

<details>
  <summary>Answer</summary>
 
```sql 
select order_id, cost * ex.rate as cost_local
from orders o join exchange_rate ex on o.country_code = ex.country_code 
and o.order_date between ex.valid_from and ex.valid_to;

select order_id, sum(cost * ex.rate) as cost_local
from orders o join exchange_rate ex on o.country_code = ex.country_code 
and o.order_date between ex.valid_from and NVL(ex.valid_to,'31-DEC-99')
group by order_id;

```
</details>  

## Get the orderid for product 'P001' and 'P002' in the same order_id

<details>
  <summary>Answer</summary>

```sql

select distinct order_id
from orders o1 join orders o2
where o1.product_id = 'P001' and o2.product_id = 'P002';

select distinct order_id
from
(
select o.*,
product_id as prod1, lag(product_id) over (partition by order_id order by product_id) as prod2
from orders o
) 
where (o.product_id = 'P001' or o.product_id = 'P002') 
 and (o.product_id = 'P002' or o.product_id = 'P001');
```
</details>

## We have a requirement to setup a data model for Online Flight booking system. There will be reporting layer will be built on top of this, which will be accessed by our customers for the analysis and reporting metrics.(Provided Expedia as example) On data modelling he was excellent. As soon I gave the problem , he was able to neatly identify all the entity, keys, relation between and explain each of those in detail.  

## Flight Booking System

```sql
create table flight
(name varchar2(100),
dep varchar2(20),
arr varchar2(20),
dep_date date,
booking_id number,
flight_no varchar2(20),
cost number
);

insert into flight values ('CustomerA', 'SEA', 'NJ', TO_DATE('2019-01-01 01:00:00','YYYY-MM-DD HH:MI:SS'), '100', 'A100', 200);
insert into flight values ('CustomerA', 'NJ', 'SEA', TO_DATE('2019-03-05 02:00:00','YYYY-MM-DD HH:MI:SS'), '100', 'A101', 200);
insert into flight values ('CustomerD', 'NJ', 'Texas', TO_DATE('2019-03-07 01:15:00','YYYY-MM-DD HH:MI:SS'), '101', 'A101', 300);
insert into flight values ('CustomerD', 'Texas', 'SEA', TO_DATE('2019-02-07 03:00:00','YYYY-MM-DD HH:MI:SS'), '101', 'A101', 100);
insert into flight values ('CustomerD', 'SEA', 'NJ', TO_DATE('2019-02-07 03:00:00','YYYY-MM-DD HH:MI:SS'), '101', 'A101', 200);
insert into flight values ('CustomerD', 'SEA', 'Texas', TO_DATE('2019-02-08 05:00:00','YYYY-MM-DD HH:MI:SS'), '102', 'A102', 400);
insert into flight values ('CustomerD', 'Texas', 'SEA', TO_DATE('2019-02-19 05:10:00','YYYY-MM-DD HH:MI:SS'), '102', 'A103', 500);
insert into flight values ('CustomerD', 'Texas', 'SEA', TO_DATE('2019-02-19 02:10:10','YYYY-MM-DD HH:MI:SS'), '103', 'A103', 200);
insert into flight values ('CustomerD', 'SEA', 'Texas', TO_DATE('2019-02-20 01:11:00','YYYY-MM-DD HH:MI:SS'), '104', 'A104', 100);
insert into flight values ('CustomerE', 'SEA', 'Texas', TO_DATE('2019-02-20 04:10:00','YYYY-MM-DD HH:MI:SS'), '105', 'A104', 200);
insert into flight values ('CustomerE', 'Texas', 'CA', TO_DATE('2019-02-21 03:10:10','YYYY-MM-DD HH:MI:SS'), '105', 'A105', 100);
insert into flight values ('CustomerF', 'SEA', 'CA', TO_DATE('2020-01-20 05:10:01','YYYY-MM-DD HH:MI:SS'), '106', 'A106', 300);
```

## List all the passengers who completed round trip within a month time

<details>
  <summary>Answer</summary>

```sql
select * 
from flight f1 join flight f2
on f1.name = f2.name and f1.booking_id = f2.booking_id
and f1.dep = f2.arr
and f1.dep_date < f2.dep_date
```
</details>

## Write a query to identify number of transits taken by passenger for each booking.
#### Expected output: booking_id, passenger_name, number_of_transits
