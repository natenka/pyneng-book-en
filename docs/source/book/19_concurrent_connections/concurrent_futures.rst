Module concurrent.futures
-------------------------

Module ``concurrent.futures`` provides a high-level interface for working with
processes and threads. For both threads and processes the same interface
is used which makes it easy to switch between them.

If you compare this module with threading or multiprocessing, it has fewer
features but with ``concurrent.futures`` it's easier to work and interface
more understandable.

Concurrent.futures module allows to solve the problem of starting multiple
threads/processes and getting data from them. For this purpose, module
uses two classes:

-  **ThreadPoolExecutor** - for threads handling
-  **ProcessPoolExecutor** - for process handling

Both classes use the same interface, so it is enough to deal with one and then
just switch to other if necessary.

Create an Executor object using ThreadPoolExecutor:

.. code:: python

    executor = ThreadPoolExecutor(max_workers=5)

After creating an Executor object, it has three methods: ``shutdown``, ``map``,
and ``submit``. Method ``shutdown`` is responsible for the completion of
threads/processes, ``map`` and ``submit`` methods are responsible for
starting functions in different threads/processes.

.. note::

    In fact, map and submit can run not only functions but any callable object.
    However, only functions will be covered further.

Method ``shutdown`` indicates that Executor object must be finished. However,
if to ``shutdown`` method pass ``wait=True`` (default value), it will not
return the result until all functions running in threads have been completed.
If ``wait=False``, ``shutdown`` method returns instantly but script itself
will not exit until all functions have been completed.

Generally, ``shutdown`` is not explicitly used because when creating an
Executor object in a context manager, ``shutdown`` is automatically called
at the end of a block with ``wait=True``.

.. code:: python

    with ThreadPoolExecutor(max_workers=5) as executor:
        ...

Since map and submit methods start a function in threads or processes, code
must at least have a function that performs one action and must be run in
different threads with different arguments of the function.

For example, if you need to ping multiple IP addresses in different threads you
need to create a function that pings one IP address and then run this function
in different threads for different IP addresses using concurrent.futures.


.. toctree::
   :maxdepth: 1

   concurrent_futures_map
   concurrent_futures_submit
   concurrent_futures_processes
