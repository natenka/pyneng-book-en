Measure script execution time 
------------------------------------

There are several options for estimating execution time of the script. The
simplest options are:

* Linux time utility
* and Python datetime module

When measuring the execution time of script in this case, high accuracy is not
important. The main thing is to compare the execution time of script in different variants.

``time``
~~~~~~~~

Linux ``time`` utility allows you to measure the execution time of a script.
To use ``time`` utility it is enough to write ``time`` before starting the script:

::

    $ time python thread_paramiko.py
    ...
    real    0m4.712s
    user    0m0.336s
    sys     0m0.064s

We are interested in real time. In this case, it's 4.7 seconds.


``datetime``
~~~~~~~~~~~~

The second option is a ``datetime`` module. This module allows working with
time and dates in Python.

Example:

.. code:: python

    from datetime import datetime
    import time

    start_time = datetime.now()

    #Code is running here
    time.sleep(5)

    print(datetime.now() - start_time)

Output:

::

    $ python test.py
    0:00:05.004949

