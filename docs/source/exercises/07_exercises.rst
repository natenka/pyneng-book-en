.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 7.1
~~~~~~~~~~~

Process the lines from the ospf.txt file and print information for each line
in this form to the stdout:

::

    Prefix                10.0.24.0/24
    AD/Metric             110/41
    Next-Hop              10.0.13.3
    Last update           3d18h
    Outbound Interface    FastEthernet0/0

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 7.2
~~~~~~~~~~~

Create a script that will process the config_sw1.txt configuration file.
The filename is passed as an argument to the script.

The script should return to the stdout commands from the passed
configuration file, excluding lines that start with '!'.

There should be no blank lines in the output.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Output example:

::

    $ python task_7_2.py config_sw1.txt
    Current configuration : 2033 bytes
    version 15.0
    service timestamps debug datetime msec
    service timestamps log datetime msec
    no service password-encryption
    hostname sw1
    interface Ethernet0/0
     duplex auto
    interface Ethernet0/1
     switchport trunk encapsulation dot1q
     switchport trunk allowed vlan 100
     switchport mode trunk
     duplex auto
     spanning-tree portfast edge trunk
    interface Ethernet0/2
     duplex auto
    interface Ethernet0/3
     switchport trunk encapsulation dot1q
     switchport trunk allowed vlan 100
     duplex auto
     switchport mode trunk
     spanning-tree portfast edge trunk
    ...


Task 7.2a
~~~~~~~~~~~~

Make a copy of the code from the task 7.2.

Add this functionality: The script should not print to the stdout commands,
which contain words from the ignore list.
The script should also not print lines that begin with !.

Check the script on the config_sw1.txt configuration file.
The filename is passed as an argument to the script.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]

Task 7.2b
~~~~~~~~~~~~

Make a copy of the code from the task 7.2a.
Add this functionality: instead of printing to stdout,
the script should write the resulting lines to a file.

File names must be passed as arguments to the script:

  1. name of the source configuration file
  2. name of the destination configuration file

In this case, the lines that are contained in the ignore list and lines
that start with ! must be filtered.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]


Task 7.3
~~~~~~~~~~~

The script should process the lines in the CAM_table.txt file. Each line,
where there is a MAC address, must be handled in such a way that
the following table was printed on the stdout:

::

    100    01bb.c580.7000   Gi0/1
    200    0a4b.c380.7000   Gi0/2
    300    a2ab.c5a0.7000   Gi0/3
    100    0a1b.1c80.7000   Gi0/4
    500    02b1.3c80.7000   Gi0/5
    200    1a4b.c580.7000   Gi0/6
    300    0a1b.5c80.7000   Gi0/7

Restriction: All tasks must be done using the topics covered in this and previous chapters.


Task 7.3a
~~~~~~~~~~~~

Make a copy of the code from the task 7.3.

Add this functionality: Sort output by VLAN number

As a result, you should get the following output:

::

    10       01ab.c5d0.70d0      Gi0/8
    10       0a1b.1c80.7000      Gi0/4
    100      01bb.c580.7000      Gi0/1
    200      0a4b.c380.7c00      Gi0/2
    200      1a4b.c580.7000      Gi0/6
    300      0a1b.5c80.70f0      Gi0/7
    300      a2ab.c5a0.700e      Gi0/3
    500      02b1.3c80.7b00      Gi0/5
    1000     0a4b.c380.7d00      Gi0/9

Pay attention to vlan 1000 - it should be displayed last.
Correct sorting can be achieved if vlan is a number, not a string.

Restriction: All tasks must be done using the topics covered in this and previous chapters.


Task 7.3b
~~~~~~~~~~~~


Make a copy of the code from the task 7.3a.

Add this functionality:

* Ask the user to enter the VLAN number.
* Print information only for the specified VLAN.

Output example:

::

    Enter VLAN number: 10
    10       0a1b.1c80.7000      Gi0/4
    10       01ab.c5d0.70d0      Gi0/8

Restriction: All tasks must be done using the topics covered in this and previous chapters.

