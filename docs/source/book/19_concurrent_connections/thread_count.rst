Number of threads
------------------

How many threads you need to use when connecting to device? There is no clear
answer to this question. The number of threads depends at least on which
computer runs the script (OS, memory, processor), on network itself (delays).

So instead of looking for the perfect number of threads, you have to measure
the number on your computer, your network, your script. For example, in the
examples to this section there is a script netmiko_count_threads.py that runs
the same function with different threads and displays runtime information.
Function by default uses a small number of devices from the devices_all.yaml
file and a small number of threads, but it can be adapted to any number based
on your network.

Example of connecting to 5,000 devices with different number of threads:

::

    Number of devices: 5460

    #30 threads
    ----------------------------------------
    Execution time: 0:09:17.187867

    #50 threads
    ----------------------------------------
    Execution time: 0:09:17.604252

    #70 threads
    ----------------------------------------
    Execution time: 0:09:17.117332

    #90 threads
    ----------------------------------------
    Execution time: 0:09:16.693774

    #100 threads
    ----------------------------------------
    Execution time: 0:09:17.083294

    #120 threads
    ----------------------------------------
    Execution time: 0:09:17.945270

    #140 threads
    ----------------------------------------
    Execution time: 0:09:18.114993

    #200 threads
    ----------------------------------------
    Execution time: 0:11:12.951247

    #300 threads
    ----------------------------------------
    Execution time: 0:14:03.790432

In this case, the execution time with 30 threads and 120 threads is the same
and after time only increases. This is because switching between threads also
takes a lot of time and the more streams the more switching. And from some moment
it makes no sense to increase number of  threads. For this example
(this PC, code and number of devices), the optimal number is approximately 50 threads.
We're not taking 30 here in order to make a reserve.


