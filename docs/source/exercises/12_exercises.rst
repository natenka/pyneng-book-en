.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 12.1
~~~~~~~~~~~~


Create a ping_ip_addresses() function that checks if IP addresses are pingable. Function expects as argument a list of IP addresses.

Function should return a tuple with two lists:

* list of reachable IP addresses
* list of unreachable IP addresses

To check availability of IP address, use ping command.

Restriction: All tasks must be performed using only covered topics.

Task 12.2
~~~~~~~~~~~~

Function ping_ip_addresses() from task 12.1 accepts only list of addresses, but it would be convenient to be able to specify addresses using a range such as 192.168.100.1-10.

In this task, you need to create a convert_ranges_to_ip_list() function that converts the list of IP addresses in different formats to a list where each IP address is specified separately.

Function expects as argument a list of IP addresses and/or IP address ranges.

List elements can be in the following format:

* 10.1.1.1
* 10.1.1.1-10.1.1.10
* 10.1.1.1-10

If address is specified as a range, you should expand range to separate addresses, including the last address of range. To simplify the task, it can be assumed that only the last octet of address changes in range.

Function returns a list of IP addresses.


For example, if you pass to convert_ranges_to_ip_list() function such a list:

.. code:: python

    ['8.8.4.4', '1.1.1.1-3', '172.21.41.128-172.21.41.132']

Function should return the list:

.. code:: python

    ['8.8.4.4', '1.1.1.1', '1.1.1.2', '1.1.1.3', '172.21.41.128',
     '172.21.41.129', '172.21.41.130', '172.21.41.131', '172.21.41.132']

Task 12.3
~~~~~~~~~~~~

Create a print_ip_table() function that displays a table of reachable and unreachable IP addresses.

Function expects as arguments two lists:

* list of reachable IP addresses
* list of unreachable IP addresses

The result of function is a table displayed on standard output:

::

    Reachable    Unreachable
    -----------  -------------
    10.1.1.1     10.1.1.7
    10.1.1.2     10.1.1.8
                 10.1.1.9

Function should not change lists passed to it as arguments. That is, lists should look the same before and after function execution.


There are no tests for this task.
