.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 4.1
~~~~~~~~~~~


Using the prepared *nat* string, get a new string that has GigabitEthernet in interface name  instead of FastEthernet. 
Restriction: All tasks must be performed using only covered topics.

.. code:: python

    nat = "ip nat inside source list ACL interface FastEthernet0/1 overload"

Task 4.2
~~~~~~~~~~~

Convert *mac* string from XXXXXX:XXXX format to XXXXXX.XXXXX.XX format.
Restriction: All tasks must be performed using only covered topics.

.. code:: python

    mac = "AAAA:BBBB:CCCC"

Task 4.3
~~~~~~~~~~~

Get from *config* string such Vlan list:

``["1", "3", "10", "20", "30", "100"]``

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    config = "switchport trunk allowed vlan 1,3,10,20,30,100"

Task 4.4
~~~~~~~~~~~

List *vlans* is a list of VLANs collected from all network devices, so list has duplicate VLAN numbers. From list you need to get a unique list of VLANs sorted in ascending order. You cannot remove specific vlans manually to get the final list.

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    vlans = [10, 20, 30, 1, 2, 100, 10, 30, 3, 4, 10]


Task 4.5
~~~~~~~~~~~

From *command1* and *command2* strings get list of VLANs that are both in *command1* and in *command2*  (intersection).

The result should be a list: ``["1", "3", "8"]``

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    command1 = "switchport trunk allowed vlan 1,2,3,5,8"
    command2 = "switchport trunk allowed vlan 1,3,8,9"

Task 4.6
~~~~~~~~~~~

Process ospf_route string and display information to standard output stream as:

::

    Prefix                10.0.24.0/24
    AD/Metric             110/41
    Next-Hop              10.0.13.3
    Last update           3d18h
    Outbound Interface    FastEthernet0/0

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    ospf_route = "       10.0.24.0/24 [110/41] via 10.0.13.3, 3d18h, FastEthernet0/0"

Task 4.7
~~~~~~~~~~~

Convert *mac* MAC-address to a binary string of this type: 
``101010101010101010111011101110111100110011001100``

Restriction: All tasks must be performed using only covered topics.

::

    mac = "AAAA:BBBB:CCCC"

Task 4.8
~~~~~~~~~~~

Convert IP address in *ip* variable to a binary format and display output in columns in this way:

* first string should be decimal bytes valuesв
* second string binary values

The output should be ordered as in example:

* by columns
* column width of 10 characters (in binary format, you have to add two spaces between columns

Example of output for address 10.1.1.1:

::

    10        1         1         1
    00001010  00000001  00000001  00000001

Restriction: All tasks must be performed using only covered topics.

::

    ip = "192.168.3.1"

