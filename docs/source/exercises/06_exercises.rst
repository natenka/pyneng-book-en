.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 6.1
~~~~~~~~~~~

Mac list contains MAC addresses in XXXXXX:XXXX:XXXX format. However, in cisco hardware, MAC addresses are used in XXXXXX.XXX.XXXX format.

Write code that converts MAC addresses to cisco format and adds them to the new list mac_cisco


Restriction: All tasks must be performed using only covered topics.

.. code:: python

    mac = ["aabb:cc80:7000", "aabb:dd80:7340", "aabb:ee80:7000", "aabb:ff80:7000"]

Task 6.2
~~~~~~~~~~~

1. Request user input of IP addresses in 10.0.1.1 format
2. Depending on address type (described below), print to standard output stream:

   * "unicast" - if first byte in range 1-223
   * "multicast" - if first byte in range 224-239
   * "local broadcast" - if IP address is 255.255.255.255
   * "unassigned" - if IP address is 0.0.0.0
   * "unused" - in all other cases

Restriction: All tasks must be performed using only covered topics.

Task 6.2a
~~~~~~~~~~~~

Make a copy of script from task 6.2.

Add a check of entered IP address. An address is considered correct if it:

* consists of 4 numbers (not letters or other symbols)
* numbers separated by a dot
* each number in range 0 to 255

If address is not set correctly, display message: "Wrong IP address". Message must be displayed only once.


Restriction: All tasks must be performed using only covered topics.

Task 6.2b
~~~~~~~~~~~~

Make a copy of script from task 6.2a.

Complete script:
If address was entered incorrectly, ask for address again.

Restriction: All tasks must be performed using only covered topics.

Task 6.3
~~~~~~~~~~~

Script has a configuration generator for access ports.

Make a similar configuration generator for trunk ports.

The situation with trunk ports is complicated by the fact that there could be many Vlans and you have to know what to do with it.

Therefore, according to each port there is a list and the first (zero) item of the list indicates how to perceive VLAN numbers that go further.

Example of value and corresponding command:

* ["add", "10", "20"] - switchport trunk allowed vlan add 10,20
* ["del", "17"] - switchport trunk allowed vlan remove 17
* ["only", "11", "30"] - switchport trunk allowed vlan 11,30

Tasks for ports 0/1, 0/2, 0/4:

* generate configuration based on template trunk_template
* based on keywords add, del, only

The code should not be tied to specific port numbers. That is, if there are other interface numbers in *trunk* dictionary, the code should work.

Restriction: All tasks must be performed using only covered topics.

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

