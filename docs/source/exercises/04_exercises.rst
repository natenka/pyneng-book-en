.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

.. note::
    In section 4, the tests can be easily "tricked" into making the
    correct output without getting results from initial data using Python.
    This does not mean that the task was done correctly, it is just that at
    this stage it is difficult otherwise test the result.

Task 4.1
~~~~~~~~~~~

Using the prepared nat string, get a new string where the FastEthernet
interface is replaced with GigabitEthernet.
Print the resulting new string to the standard output (stdout) using print.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    nat = "ip nat inside source list ACL interface FastEthernet0/1 overload"

Task 4.2
~~~~~~~~~~~

Convert string in mac variable from XXXX:XXXX:XXXX format to
XXXX.XXXX.XXXX format.
Print the resulting new string to the standard output (stdout) using print.

Restriction: All tasks must be done using the topics covered in this and previous
chapters.

.. code:: python

    mac = "AAAA:BBBB:CCCC"

Task 4.3
~~~~~~~~~~~

Get the following list of VLANs from the config string:
``["1", "3", "10", "20", "30", "100"]``


Write the resulting list to the result variable.
(this is the variable that will be checked in the test)

Print the resulting list to the standard output (stdout) using print.

Here is a very important point that you need to get exactly the list (data type),
and not, for example, a string that looks like the list shown.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    config = "switchport trunk allowed vlan 1,3,10,20,30,100"

Task 4.4
~~~~~~~~~~~

Vlans list is a list of VLANs collected from all devices on the network,
therefore there are duplicate VLAN numbers in the list.

Get a new list of unique VLAN numbers from the vlans list,
sorted in ascending order of numbers. To get the final
list, you cannot delete specific vlans manually.

Write the resulting list to the result variable.
(this is the variable that will be checked in the test)

Print the resulting list to the standard output (stdout) using print.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    vlans = [10, 20, 30, 1, 2, 100, 10, 30, 3, 4, 10]


Task 4.5
~~~~~~~~~~~

From the strings command1 and command2, get a list of VLANs that exist
in both command1 and command2 (intersection).

In this case, the result should be a list: ``['1', '3', '8']``.

Write the resulting list to the result variable.
(this is the variable that will be checked in the test)

Print the resulting list to the standard output (stdout) using print.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    command1 = "switchport trunk allowed vlan 1,2,3,5,8"
    command2 = "switchport trunk allowed vlan 1,3,8,9"

Task 4.6
~~~~~~~~~~~

Process the ospf_route string and print the information to the stdout as follows:

::

    Prefix                10.0.24.0/24
    AD/Metric             110/41
    Next-Hop              10.0.13.3
    Last update           3d18h
    Outbound Interface    FastEthernet0/0

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    ospf_route = "       10.0.24.0/24 [110/41] via 10.0.13.3, 3d18h, FastEthernet0/0"

Task 4.7
~~~~~~~~~~~

Convert MAC address in mac string to binary string like this:
``101010101010101010111011101110111100110011001100``

Print the resulting new string to the standard output (stdout) using print.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

::

    mac = "AAAA:BBBB:CCCC"

Task 4.8
~~~~~~~~~~~

Convert the IP address in the ip variable to binary and print output in columns
to stdout:

* the first line must be decimal values
* the second line is binary values

The output should be ordered in the same way as in the example output below:

* in columns
* column width 10 characters (in binary
  you need to add two spaces between columns
  to separate octets among themselves)

Example output for address 10.1.1.1:

::

    10        1         1         1
    00001010  00000001  00000001  00000001

Restriction: All tasks must be done using the topics covered in this and previous chapters.

::

    ip = "192.168.3.1"

