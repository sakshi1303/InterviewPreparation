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

## Slowly Changing Dimension Attributes

1. Type 0: Keep Original and do not change anything.
2. Type 1: Overwrite and update the original values.
3. Type 2: Maintain History using 3 arributes - effective from date, effective to date and current indicator.
4. Type 3: Type 2 along with an additional column to previous/latest values.
5. Type 4: When few dimensions are frequently changing then split them to mini-dimension. Primay keys of both the dimensions are foreign keys to associated facts.
6. Type 5: Adding a Type 1 reference to mini-dimension in the base dimension(Type 4 + Type 1). A separate table/view in presentation layer which contains both mini & base dimension.
7. Type 6: Type 2 along another column for storing current values like Type 3.
8. Type 7: Hybrid technique for Type 5 or Type 6 views. When current values are considered, join fact table with durable key and for historical values, join fact table with surrogate primary key. These act as two separate views to BI applications.

## Dealing with Positional Hierarchies

1. Fixed Depth Positional Hierarchies - A fixed depth hierarchy is a series of many-to-one relationships, such as product to brand to category to department. This uses a separate positional attributes in dimension table for fast query performance.
2. Slightly Ragged/Variable Depth Hierarchies - Introduce separate dimension attributes for maximum number of levels to convert it to fixed-depth positional hierarchy.
3. Ragged/Variable Depth Hierarchies with Hierarchy Bridge Tables - Add specially designed bridge table which contains record for every possible path/level in ragged hierarchy.
4. Ragged/Variable Depth Hierarchies with Pathstring Attributes - Do not use bridge table rather add a pathstring attribute which contains complete path description from highest node down to the node described by that particular dimension row.

## Advanced Fact Table Techniques

1. Fact Table surrogate keys can be useful but not mandatory. Sequential and not associated with any dimension.
2. Centipede Fact Tables result when low cardinal foreign keys are used in fact tables instead of junk dimension. 
3. If Numeric Values are used for calculation purposes then use in Fact and if they are to be filtered/grouped then use in Dimensions.
4. Accumulating Facts should store one time lag for each step from starting point so that lag between consecutive steps can be calculated.
5. All header-level dimension foreign keys and degenerate dimensions should be included in line-level fact table.
6. Allocated facts can be sliced and rolled up by all dimensions.
7. Profit and Loss Fact Tables Using Allocations at atomic transaction grain.
8. Multiple Currency Facts to be maintained for transactions occuring at true currency and in standard currency.
9. Store the standard measure and conversion factor in single fact table in case of Multiple Units of measure and provide a view to users containing the other units as per the needs.
10. YTD should be handled at BI layer rather than storing in Fact Tables.
11. Never join two Fact Tables , instead use drilling across technique and sort-merge them later.
12. SCD Type 2 in Fact Tables to capture timespan.
13. Late Arriving Facts if most current dimensional context does not match incoming row.

## Advanced Dimension Table Techniques

1. Dimension-to-Dimension Table Joins should be avoided instead place foreign key in fact table as it can cause excessive growth of one or the other dimension 
2. Multivalued Dimensions must be attached to fact table using group dimension key to bridge tables. For example a patient diagnosed with multiple ailments.
3. A multivalued bridge table to be used with SCD Type 2. 
4. Behavior Tag Time Series
5. Behavior Study Groups which consists only of customers durable key.
6. Aggregated Facts as dimension attributes such as filtering on all customers who spent over a certain amount.
7. Dynamic Value Bands is putting varying size range in a dimension and to be implemented at report level instead during ETL processing. 
8. Freeform texts to be stored in dimensions with coressponding foreign key in fact table.
9. Multiple time zones should be stored in fact table.
10. Measure type dimensions generally not recommended.
11. Step dimension is used at determine current step and how many steps are required to complete the session.
12. Hot swappable dimensions are used when the same fact table is alternatively paired with different copies of the same dimension.
13. Abstract generic dimension should be avoided in dimensional modeling.
14. Audit dimensions are metadata columns like insert/update time esp. useful for compliance and auditing purpose.
15. Late arriving dimensions means dimensional context has not arrived and these dimension row's has attributes that are specified as UNKNOWN. SCD Type 1 changes are done when dimensional context arrives.

## Special Purpose Schemas

1. Supertype and Subtype Schemas for Heterogeneous Products
2. Real-Time Fact Tables
3. Error event schemas where each error is the grain to maintain data quality in a datawarehouse system.
