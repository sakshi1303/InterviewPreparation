## Project 1 - Speeding Up Data using Multithreading

1. Conventional approach for data load using INSERT INTO/CREATE TABLE AS works well with data size in MB but when it comes to datasize of GB/TB, then the approach is not suitable.
2. Performance degrades to a great extent and hence the approach cannot be accepted in a real world scenario wherein we deal with billions of data.
3. In order to achieve the optimum performance/best results, we need to take un-conventional approach, one of which is parallel processing.
4. Python's multithreading module initiates multiple parallel thread execution which not only takes less memory because of shared resources but also helps achieve great performance because of concurrent execution of processes.
5. Along with Multithreading, Queue module of Python manages the concurrent execution of these threads so that information can be safely exchanged between them.
6. Together with these modules, we can speed up the processing of large volumes of data and achieve best results in terms of performance. It is also less prone to errors like memory issues etc.
7. Example case and Python code can be accessed from https://github.com/sakshi1303/Python/blob/master/MultiThreading/MultiThreading.md

## Project 2 - Netting Example

#### Situation - 

## Project 3 - Materialised Views

1. Situation was Like Search on one of application functionality was taking more time than expected i.e 4-5 mins for a single search.
2. Task was to improve the performance and get the results within milliseconds.
3. Action taken were -
   > - Created Function based Index on related column
   > - Created a Global Temporary Table to insert data at runtime and fetch the results from the same.
   > - When above approach didn't work, we analysed the table and found that there are around 250+ columns in the table and we required only few. We then created a Materialised View only required columns and used it to fetch the results.
4. Result was that we were able to reduce the performance to a great extent and got the results within milliseconds.
