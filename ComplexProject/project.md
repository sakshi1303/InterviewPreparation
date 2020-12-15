## Project 1 - Speeding Up Data using Multithreading

1. Conventional approach for data load using INSERT INTO/CREATE TABLE AS works well with data size in MB but when it comes to datasize of GB/TB, then the approach is not suitable.
2. Performance degrades to a great extent and hence the approach cannot be accepted in a real world scenario wherein we deal with billions of data.
3. In order to achieve the optimum performance/best results, we need to take un-conventional approach, one of which is parallel processing.
4. Python's multithreading module initiates multiple parallel thread execution which not only takes less memory because of shared resources but also helps achieve great performance because of concurrent execution of processes.
5. Along with Multithreading, Queue module of Python manages the concurrent execution of these threads so that information can be safely exchanged between them.
6. Together with these modules, we can speed up the processing of large volumes of data and achieve best results in terms of performance. It is also less prone to errors like memory issues etc.
7. Example case and Python code can be accessed from https://github.com/sakshi1303/Python/blob/master/MultiThreading/MultiThreading.md

STAR Format -

1. Situation was to load around 10 TB of data and apply transformations on it. The initial process implemented took 1.5 hours for one effective day, 1 year was expected around 1 month and total estimation on 10 years of data was 10 months approx.
2. Task was to improve the performance and bring it down to a week's time.
3. Action taken were -
   > - Use of Python's multithreading module along with Queue module to achieve concurrency.
   > - One day load was completing in 20 mins (from 1.5 hrs)
   > - On analysing further, we found that there were too many INSERT INTO SELECT statements which was then replaced with CREATE TABLE AS stmts using dynamic sql's.
   > - Now, the load was taking approx 5 mins and 1 year data completed in approx. 30 hours.
 4. Result was that we were able to achieve and complete the load in 8 days.  

## Project 2 - Netting Example

1. Situation was one of our Monthly Reporting process used to take around 24 hours to complete the overall activity which comprised of 15-20 complex sql queries.
2. Task was to improve the performance and get the results within 1 hour.
3. Action taken were -
   > - Gathered Statistics on all the tables
   > - Removing Fragmentation from all the related tables
   > - Applied GATHER STATS in all intermediate steps as stats were gathered with 0 records but there were millions of records actually while the process ran to load data in Staging Tables hence making the queries slower.
   > - We were able to reduce the performance by 50% but it was not up to the mark.
   > - We further analysed that there were lot of INSERT INTO statements were used which is slower hence we replaced them with CREATE TABLE AS statements using dynamic sql's.
 4. Result was that we were able to achieve the performance within expected time which is 1 hour.  

## Project 3 - Materialised Views

1. Situation was Like Search on one of application functionality was taking more time than expected i.e 4-5 mins for a single search.
2. Task was to improve the performance and get the results within milliseconds.
3. Action taken were -
   > - Created Function based Index on related column
   > - Created a Global Temporary Table to insert data at runtime and fetch the results from the same.
   > - When above approach didn't work, we analysed the table and found that there are around 250+ columns in the table and we required only few. We then created a Materialised View only required columns and used it to fetch the results.
   > - Because Oracle has row-based storage unlike Redshift so it does a FULL TABLE scan with all the columns hence taking more time.
4. Result was that we were able to reduce the performance to a great extent and got the results within milliseconds.
