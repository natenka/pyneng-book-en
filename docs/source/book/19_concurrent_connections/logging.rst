Module logging
--------------

Module logging - a module from the standard Python library that allows you to configure logging from the script. Module logging has a lot of features and a lot of configuration options. Only basic configuration option is discussed in this section.

The easiest way to configure logging in script, use logging.basicConfig:

.. code:: python

    import logging


    logging.basicConfig(
        format='%(threadName)s %(name)s %(levelname)s: %(message)s',
        level=logging.INFO)

In this variant, the settings are:

* all messages will be displayed on standard output, 
* messages of INFO level and above will be displayed, 
* each message will contain thread information, log name, message level, and message itself.

Now, to output a log message in this script, you should write  ``logging.info("test")``.

Example of script with logging settings: (logging_basics.py file)

.. literalinclude:: /pyneng-examples-exercises/examples/20_concurrent_connections/logging_basics.py
  :language: python
  :linenos:

Result of script execution:

::

    $ python logging_basics.py
    MainThread root INFO: ===> 12:26:12.767168 Connection: 192.168.100.1
    MainThread root INFO: <=== 12:26:18.307017 Received:   192.168.100.1
    *12:26:18.137 UTC Wed Jun 5 2019
    MainThread root INFO: ===> 12:26:18.413913 Connection: 192.168.100.2
    MainThread root INFO: <=== 12:26:23.991715 Received:   192.168.100.2
    *12:26:23.819 UTC Wed Jun 5 2019
    MainThread root INFO: ===> 12:26:24.095452 Connection: 192.168.100.3
    MainThread root INFO: <=== 12:26:29.478553 Received:   192.168.100.3
    *12:26:29.308 UTC Wed Jun 5 2019

.. note::

    There are still many features in logging module. This section only uses basic configuration option. For more information on features of the module, see `Logging HOWTO <https://docs.python.org/3/howto/logging.html#logging-basic-tutorial>`__
