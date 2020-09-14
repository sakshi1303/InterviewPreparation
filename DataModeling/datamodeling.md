## Normal Forms

Normalization is a process of organizing the data in database to avoid data redundancy, insertion anomaly, update anomaly & deletion anomaly. 
For example - Below is the employee table 

|emp_id|emp_name|emp_address|emp_dept|
|----|----|----|----|
|101|Rick|Delhi|D001|
|101|Rick|Delhi|D002|
|123|Maggie|Agra|D890|
|166|Glenn|Chennai|D900|
|166|Glenn|Chennai|D004|

1. Update Anomaly - When an update happens on emp_id = 101 then it is required to be done for all the corresponding records else it will cause inconsistencies. If by any chance, only one record gets updated then there will be different addresses in twp departments for emp 101 which would lead to inconsistent data.
2. Insertion Anomaly - If an employee is hired and is under training with no department assigned, then we would have to insert emp_dept if it has a NOT NULL constraint. 
3. Deletion Anomaly - If an employee resigns and is a single resource in a department then we would loose the department information as well.

Below are the commonly used Normal Forms -


> - First normal form(1NF)
> - Second normal form(2NF)
> - Third normal form(3NF)
> - Boyce & Codd normal form (BCNF)

#### First Normal Form (1 NF)

According to first normal form rule, an attribute (column) of a table cannot hold multiple values. It should hold only atomic values.
For example , the below table is not in 1NF-

|emp_id|emp_name|emp_address|emp_mobile|
|----|----|----|----|
|101|Rick|Delhi|8912312390|
|101|Rick|Delhi|8812121212<br/>9900012222|
|123|Maggie|Agra|7778881212|
|166|Glenn|Chennai|9990000123<br/>8123450987|
|166|Glenn|Chennai|8123450987|

After normalising the table to 1NF -

|emp_id|emp_name|emp_address|emp_mobile|
|----|----|----|----|
|101|Rick|Delhi|8912312390|
|101|Rick|Delhi|8812121212|
|101|Rick|Delhi|9900012222|
|123|Maggie|Agra|7778881212|
|166|Glenn|Chennai|9990000123|
|166|Glenn|Chennai|8123450987|
|166|Glenn|Chennai|8123450987|
