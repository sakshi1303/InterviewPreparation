## Project 1 - Speeding Up Data using Multithreading

1. Conventional approach for data load using INSERT INTO/CREATE TABLE AS works well with data size in MB but when it comes to datasize of GB/TB, then the approach is not suitable.
2. Performance degrades to a great extent and hence the approach cannot be accepted in a real world scenario wherein we deal with billions of data.
3. In order to achieve the optimum performance/best results, we need to take un-conventional approach, one of which is parallel processing.
4. Python's multithreading module initiates multiple parallel thread execution which not only takes less memory because of shared resources but also helps achieve great performance because of concurrent execution of processes.
5. Along with Multithreading, Queue module of Python manages the concurrent execution of these threads so that information can be safely exchanged between them.
6. Together with these modules, we can speed up the processing of large volumes of data and achieve best results in terms of performance and is prone to errors like memory issues etc.
7. Example case and Python code can be accessed using https://github.com/sakshi1303/Python/blob/master/MultiThreading/MultiThreading.md
