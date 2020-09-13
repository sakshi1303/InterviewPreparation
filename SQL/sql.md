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
