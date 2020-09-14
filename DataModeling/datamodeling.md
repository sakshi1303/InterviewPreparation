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

#### Second Normal Form (2 NF)

A table is said to be in 2NF if - 

> - Table is in 1NF
> - No non-prome attribute should be dependent on proper subset of any candidate key of table.

For example - 

|teacher_id|subject|teacher_age|
|----|---|----|
|111|Maths|38|
|111|Physics|38|
|222|Biology|38|
|333|Physics|40|
|333|Chemistry|40|

Candidate Keys: {teacher_id, subject}<br/>
Non prime attribute: teacher_age

Here, teacher_age is dependent on teacher_id alone(but not on subject) which is a proper subset of candidate key. Hence this is not in 2NF.

After normalising the table to 2NF -

teacher_details :-
|teacher_id|teacher_age|
|----|----|
|111|38|
|222|38|
|333|40|

teacher_subject :-
|teacher_id|subject|
|----|---|
|111|Maths|
|111|Physics|
|222|Biology|
|333|Physics|
|333|Chemistry|

#### Third Normal Form (3 NF)

A table is said to be in 3NF if - 

> - Table is in 2NF
> - Transitive functional dependency of non-prime attribute on any super key should be removed.

An attribute that is not part of any candidate key is known as non-prime attribute.

In other words 3NF can be explained like this: A table is in 3NF if it is in 2NF and for each functional dependency X-> Y at least one of the following conditions hold:

> - X is a super key of table
> - Y is a prime attribute of table

An attribute that is a part of one of the candidate keys is known as prime attribute.

For example - 

|emp_id|emp_name|emp_zip|emp_state|emp_city|emp_district|
|----|-----|-----|----|----|----|
|1001|John|282005|UP|Agra|Dayal Bagh|
|1002|Ajeet|222008|TN|Chennai|M-City|
|1006|Lora|282007|TN|Chennai|Urrapakkam|
|1101|Lilly|292008|UK|Pauri|Bhagwan|
|1201|Steve|222999|MP|Gwalior|Ratan|

Super keys: {emp_id}, {emp_id, emp_name}, {emp_id, emp_name, emp_zip}…so on<br/>
Candidate Keys: {emp_id}<br/>
Non-prime attributes: all attributes except emp_id are non-prime as they are not part of any candidate keys.

emp_id -> emp_zip<br/>
emp_zip -> emp_state,emp_city,emp_district

This means, emp_state/emp_city/emp_district are transitively dependant on emp_id as - 

emp_id -> emp_zip -> emp_state

Here, emp_state, emp_city & emp_district dependent on emp_zip. And, emp_zip is dependent on emp_id that makes non-prime attributes (emp_state, emp_city & emp_district) transitively dependent on super key (emp_id). This violates the rule of 3NF.

To make this table complies with 3NF we have to break the table into two tables to remove the transitive dependency:

employee table :-
|emp_id|emp_name|emp_zip|
|---|---|---|
|1001|John|282005|
|1002|Ajeet|222008|
|1006|Lora|282007|
|1101|Lilly|292008|
|1201|Steve|222999|

employee_zip table :-
|emp_zip|emp_state|emp_city|emp_district|
|---|---|---|----|
|282005|UP|Agra|Dayal Bagh|
|222008|TN|Chennai|M-City|
|282007|TN|Chennai|Urrapakkam|
|292008|UK|Pauri|Bhagwan|
|222999|MP|Gwalior|Ratan|

#### Boyce Codd Normal Form (BCNF)

A table is said to be in bcnf/3.5NF if - 

> - Table is in 3NF
> - For every functional dependency X->Y, X should be the super key of the table.

|emp_id|emp_nationality|emp_dept|dept_type|dept_no_of_emp|
|---|----|-----|----|----|
|1001|Austrian|Production and planning|D001|200|
|1001Austrian|stores|D001|250|
|1002|American|design and technical support|D134|100|
|1002|American|Purchasing department|D134|600|

Functional dependencies in the table above:<br/>
emp_id -> emp_nationality<br/>
emp_dept -> {dept_type, dept_no_of_emp}

Candidate key: {emp_id, emp_dept}

The table is not in BCNF as neither emp_id nor emp_dept alone are keys.

To make the table comply with BCNF we can break the table in three tables like this:

emp_nationality table:
|emp_id|emp_nationality|
|---|----|
|1001|Austrian|
|1002|American|

emp_dept table:
|emp_dept|dept_type|dept_no_of_emp|
|----|----|----|
|Production and planning|D001|200|
|stores|D001|250|
|design and technical support|D134|100|
|Purchasing department|D134|600|

emp_dept_mapping table:
|emp_id|emp_dept|
|---|----|
|1001|Production and planning|
|1001|stores|
|1002|design and technical support|
|1002|Purchasing department|

Functional dependencies:<br/>
emp_id -> emp_nationality<br/>
emp_dept -> {dept_type, dept_no_of_emp}

Candidate keys:<br/>
For first table: emp_id<br/>
For second table: emp_dept<br/>
For third table: {emp_id, emp_dept}

This is now in BCNF as in both the functional dependencies left side part is a key.
