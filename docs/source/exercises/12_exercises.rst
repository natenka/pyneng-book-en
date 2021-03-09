.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 12.1
~~~~~~~~~~~~

Create a ping_ip_addresses function that checks if IP addresses are pingable.

The function expects a list of IP addresses as an argument.

The function must return a tuple with two lists:

* list of available IP addresses
* list of unavailable IP addresses

To check the availability of an IP address, use the ping command.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 12.2
~~~~~~~~~~~~

The ping_ip_addresses function from task 12.1 only accepts a list of addresses,
but it would be convenient to be able to specify addresses using a range,
for example 192.168.100.1-10.

In this task, you need to create a function convert_ranges_to_ip_list that
converts a list of IP addresses in different formats into a list where each
IP address is listed separately.

The function expects as an argument a list containing IP addresses
and/or ranges of IP addresses.

List items can be in the format:

* 10.1.1.1
* 10.1.1.1-10.1.1.10
* 10.1.1.1-10

If the address is specified as a range, the range must be expanded into
individual addresses, including the last address in the range.
To simplify the task, we can assume that only the last octet of the
address changes in the range.

The function returns a list of IP addresses.

For example, if you pass the following list to the convert_ranges_to_ip_list function:

.. code:: python

    ['8.8.4.4', '1.1.1.1-3', '172.21.41.128-172.21.41.132']

The function should return a list like this:

.. code:: python

    ['8.8.4.4', '1.1.1.1', '1.1.1.2', '1.1.1.3', '172.21.41.128',
     '172.21.41.129', '172.21.41.130', '172.21.41.131', '172.21.41.132']


Task 12.3
~~~~~~~~~~~~

Create a function print_ip_table that prints a table of available
and unavailable IP addresses.

The function expects two lists as arguments:

* list of available IP addresses
* list of unavailable IP addresses

The result of the function is printing a table to the stdout:

::

    Reachable    Unreachable
    -----------  -------------
    10.1.1.1     10.1.1.7
    10.1.1.2     10.1.1.8
                 10.1.1.9
