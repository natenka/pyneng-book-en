.. raw:: latex

   \newpage

.. _concurrent_connections_index:

19. Concurent connections to multiple devices
======================================================

When you need to connect to a large number of devices, it will take quite
a long time to complete the connections one by one. Of course, it will be
faster than manual connection, but it would be faster to connect to equipment in parallel.

Module concurrent.futures is used for parallel connection to devices
in this section.

.. note::

    All these "long" and "faster" are relative concepts, but in this section
    we will learn to measure exact script execution time to compare how quick
    the connection is established.

.. toctree::
   :maxdepth: 1

   measure_script_execution_time
   cpython_gil
   thread_count
   thread_safety
   logging
   concurrent_futures
   further_reading
   ../../exercises/19_exercises
