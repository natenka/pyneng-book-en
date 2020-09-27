.. raw:: latex

   \newpage

.. _concurrent_connections_index:

19. Concurent connections to multiple devices
======================================================

When you have to poll many devices, the connections will take quite a long time to connect in turn. Of course, this will be faster than manual connection but we’d like to get response as soon as possible.

.. note::

    All these "long" and "faster" are relative concepts, but in this section we will learn to measure exact script execution time to compare how quick the connection is established.

Module concurrent.futures is used for parallel connection to devices in this section.

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
