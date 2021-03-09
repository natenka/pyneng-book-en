.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 6.1
~~~~~~~~~~~

The mac list contains MAC addresses in the format XXXX:XXXX:XXXX
However, in Cisco equipment MAC addresses are in XXXX.XXXX.XXXX format.

Write a code that converts MAC addresses to cisco format and adds them to
a new list named result. Print the result list to the stdout using print function.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    mac = ["aabb:cc80:7000", "aabb:dd80:7340", "aabb:ee80:7000", "aabb:ff80:7000"]

Task 6.2
~~~~~~~~~~~

Prompt the user to enter an IP address in the format 10.0.1.1.
Depending on the type of address (described below), print to the stdout:

* 'unicast' - if the first byte is in the range 1-223
* 'multicast' - if the first byte is in the range 224-239
* 'local broadcast' - if the IP address is 255.255.255.255
* 'unassigned' - if the IP address is 0.0.0.0
* 'unused' - in all other cases

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 6.2a
~~~~~~~~~~~~

Make a copy of the code from the task 6.2.

Add verification of the entered IP address.
An IP address is considered correct if it:

* consists of 4 numbers (not letters or other symbols)
* numbers are separated by a dot
* every number in the range from 0 to 255

If the IP address is incorrect, print the message: 'Invalid IP address'

The message "Invalid IP address" should be printed only once,
even if several points above are not met.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 6.2b
~~~~~~~~~~~~

Make a copy of the code from the task 6.2a.

Add this functionality: If the address was entered incorrectly, request the address again.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 6.3
~~~~~~~~~~~

A configuration generator for access ports is made in the script.
Make a similar configuration generator for trunk ports.

In trunks, the situation is complicated by the fact that there can be many VLANs,
and you need to understand what to do with them (add, delete, overwrite).

Therefore, in accordance with each port there is a list and the first (zero index)
element of the list specifies how to interpret VLAN numbers that follow.


Dict value and corresponding command:

* ``['add', '10', '20']`` - switchport trunk allowed vlan add 10,20
* ``['del', '17']`` - switchport trunk allowed vlan remove 17
* ``['only', '11', '30']`` - switchport trunk allowed vlan 11,30

Task for ports 0/1, 0/2, 0/4:

* generate a configuration based on the trunk_template template
* taking into account the keywords add, del, only

The code should not be tied to specific port numbers. I.e,
if there are other interface numbers in the trunk dictionary, the code should work.

For data in the trunk_template dictionary, output to
the standard output should be like this:

::

    interface FastEthernet 0/1
     switchport trunk encapsulation dot1q
     switchport mode trunk
     switchport trunk allowed vlan add 10,20
    interface FastEthernet 0/2
     switchport trunk encapsulation dot1q
     switchport mode trunk
     switchport trunk allowed vlan 11,30
    interface FastEthernet 0/4
     switchport trunk encapsulation dot1q
     switchport mode trunk
     switchport trunk allowed vlan remove 17

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    access_template = [
        "switchport mode access",
        "switchport access vlan",
        "spanning-tree portfast",
        "spanning-tree bpduguard enable",
    ]

    trunk_template = [
        "switchport trunk encapsulation dot1q",
        "switchport mode trunk",
        "switchport trunk allowed vlan",
    ]

    access = {"0/12": "10", "0/14": "11", "0/16": "17", "0/17": "150"}
    trunk = {"0/1": ["add", "10", "20"], "0/2": ["only", "11", "30"], "0/4": ["del", "17"]}

    for intf, vlan in access.items():
        print("interface FastEthernet" + intf)
        for command in access_template:
            if command.endswith("access vlan"):
                print(f" {command} {vlan}")
            else:
                print(f" {command}")

