## 4 Step Dimensional process

1. Gather business requirement
2. Declare grain
3. Declare facts
4. Declare dimensions.

## Basic Fact Table Techniques

1. Facts are numeric data which can be measured.
2. Fact Tables contains foreign keys from its associated dimensions along with degenerate dimension keys and date/time stamps.
3. Additive Facts can be summed across all records eg. credit/debit amount, semi-additive which can be summed partially eg. balance cannot be summed and non-additive cannot be summed at all eg. ratios etc.
4. NULLs in Fact Tables must be avoided instead use UNKNOWN/NOT APPLICABLE to avoid refrential integrity constraint if any.
5. Conformed Facts are stored in different fact tables and their technical definitons are identical if they are to be compared together.
6. Transaction Facts correspondes to a measurement event at a point in space and time which may be dense or sparse because rows exist only if a measurement takes place.
7. Periodic Snapshot Facts are facts over a standard period such as day, week and month and is uniformly dense as the row is inserted even if no activity takes place.
8. Accumulating Snapshot Fact Tables are the measurement events which occur at predictable steps between the beginning and end of the process. For example Pipeline or workflow process such as order fulfillment or claim processing that has defined starting point, standard intemediate steps and defined end point. Records are initially inserted and as and when the pipeline progress, rows are revisted and updated.
9. Factless Fact Table are those facts when there are no actual measurements but facts are still present. For example a student attending a class everyday. It can be used to analyse what did not happen. It has two parts - coverage facts which has all the possiblities that might happen and activity table when the events did happen.
10. Aggregated Facts are numeric rollup of atomic facts to accelerate query performance and directly available to BI layer. For example, total students in a class/school. This process is known as aggregate navigation.
11. Consolidated Facts are those when muliple processes combine together to a single consolidated fact used to analyse/compare. For eg - comparing actual & predicted sales. It adds burden to ETL layer but ease the analytic burden.

## Basic Dimension Table Techniques

1. Dimension Table has single primary key column and acts as a foreign key to associated fact table. They are flat denormalised tables and the dimension attributes are populated with verbose descriptions.
2. Dimension Surrogate Keys are primary key columns for dimensions which cannot be operational system's natural key because the natural keys can change over time. Also, they can be created by more than one system which may be incompatible or poorly administered. Hence, it is suggested to use surrogate keys which are sequentially generated.
3. Natural keys are created by operational source systems which are subject to business rules outside the control of DW/BI systems. For example, a employee resigns and can be rehired. So, a new durable key must be created that is persistant and does not change with such situations. This key is also referred to as durable supernatural key.
4. Drilling Down is adding a row header to an existing query which is appended to GROUP BY clause. Example - a school, student and class entity.
5. Degenerate Dimensions has no associated dimension table. Eg. invoice number but it is a valid dimension key for fact tables at the line level item. These are most common with transaction and accumulating snapshot fact tables.
6. Denormalised Flattened Dimension are denormalised, contains many-to-one fixed depth hierarchies into separate attributes on a flattened dimension row which supports simplicity and speed.
7. Multiple Hierarchies can co-exist in one dimension. Example day to week, day to month, geographical hierarchies.
8. Flags, Codes and Operational indicators should have complete meaning in full text defined in dimensions.
9. NULL's must be avoided in Dimensions to avoid constraints like NOT NULL/Refrential Integrity. Instead use UNKNOWN/NOT APPLICABLE codes.
10. Calendar Date Dimensions contains date attributes like day/week/month date, day, number, fiscal period etc and it does not act as foreign key to facts. Calendar date key to be used as YYYYMMDD for partitioning instead of a surrogate key.
11. One dimension can be used as a reference in a fact table multiple times with different logical dimensions are called as Role Playing Dimensions. Ex - Date Dimensions.
12. Various low cardinality flags or indicators can be grouped together and placed in a single JUNK dimensions rather than having separate dimemsions for each of them. This is also known as Transaction Profile Dimension.
13. Snowflake dimension is normalised hieararchical multilevel dimensions which can negatively impact performance and contains exactly same information as that of flattened denormalised dimensions.
14. Outtrigger dimension contains reference to another dimension table. Eg. bank account dimension can reference another dimension containing date when account was opened.
