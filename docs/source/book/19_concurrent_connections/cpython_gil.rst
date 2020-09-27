Processes and threads in Python (CPython)
------------------------------------

First, we need to work out the terms:

-  process - roughly speaking, it's a launched program. Separate resources are allocated to the process: memory, processor time
-  thread - execution unit in the process. Thread share resources of the process to which they relate.

Python (or, more precisely, Cpython - the implementation used in the book)
is optimized to work in single-threaded mode. This is good if program uses only one thread. And, at the same time, Python has certain nuances of running in multithreaded mode. This is because Cpython uses GIL (global interpreter lock).

GIL does not allow multiple threads to execute Python code at the same time. If you don’t go into detail, GIL can be visualized as a sort of flag that carried over from thread to thread. Whoever has the flag can do the job. The flag is transmitted either every Python instruction or, for example, when some type of input-output operation is performed.

Therefore, different threads will not run in parallel and the program will simply switch between them executing them at different times. However, if in the program there is some "wait" (packages from the network, user request, time.sleep pause), then in such program the threads will be executed as if in parallel. This is because during such pauses the flag (GIL) can be passed to another thread.

That is, threads are well suited for tasks that involve input-output operations:

* Connection to equipment and network connectivity in general
* Working with file system
* Downloading files

.. note::

    In the Internet it is often possible to find phrases like «In Python it is better not to use threads at all». Unfortunately, such phrases are not always written in context, namely that it is about specific tasks that are tied to CPU.


The next sections discuss how to use threads to connect via Telnet/SSH. Script execution time will be checked comparing the sequential execution and execution using processes.

Processes 
~~~~~~~~

Processes allow to execute tasks on different computer cores. This is important for tasks that are tied to CPU. For each process a copy of resources is created, a memory is allocated, each process has its own GIL. This also makes processes heavier than threads.

In addition, the number of processes that run in parallel depends on the number of cores and CPU and is usually estimated in dozens, while the number of threads for input-output operations can be estimated in hundreds.

Processes and threads can be combined but this complicates the program and at the base level for input-output operations it is better to stop at threads.

.. note::
    
    Combining threads and processes, i.e., starting a process in a program and then starting threads inside it, makes troubleshooting difficult. And I’d rather not use that option.


Although it is usually better to use threads for input-output tasks, for some modules it is better to use processes because they may not work correctly with threads.

.. note::
    
    In addition to processes and threads, there is another variant of concurrent connections to device: asynchronous programming. This option is not discussed in the book..

