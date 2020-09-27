.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 7.1
~~~~~~~~~~~

Process lines from ospf.txt file and display information for each line as follows:

::

    Prefix                10.0.24.0/24
    AD/Metric             110/41
    Next-Hop              10.0.13.3
    Last update           3d18h
    Outbound Interface    FastEthernet0/0

Restriction: All tasks must be performed using only covered topics.

Task 7.2
~~~~~~~~~~~

Create a script that will process configuration file config_sw1.txt. The file name is passed as a  script argument.

Script should return commands from passed configuration file, excluding lines that start with ``!``.

Output should be without empty lines.

Restriction: All tasks must be performed using only covered topics.

Task 7.2a
~~~~~~~~~~~~

Make a copy of script from task 7.2.

Complete script: Script should not display commands containing words that are specified in *ignore* list.

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]

Task 7.2b
~~~~~~~~~~~~

Complete script from task 7.2a: instead of displaying to standard output stream, script should write received lines to config_sw1_cleared.txt file

You have to filter lines from *ignore* list.
Lines that start with  ``!`` should not be filtered.

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]

Task 7.2c
~~~~~~~~~~~~

Redo script from task 7.2b: pass to script as arguments:

* source configuration file name
* resulting configuration file name

Inside, script should filter those lines in original configuration file that contain words from *ignore* list. And write the rest of lines to resulting file.

Check script with config_sw1.txt.

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]

Task 7.3
~~~~~~~~~~~

Script should process entries in CAM_table.txt file. Every line with MAC address should be processed in a way that such view table is displayed on standard output stream (not all lines from the file are shown):

::

    100    01bb.c580.7000   Gi0/1
    200    0a4b.c380.7000   Gi0/2
    300    a2ab.c5a0.7000   Gi0/3
    100    0a1b.1c80.7000   Gi0/4
    500    02b1.3c80.7000   Gi0/5
    200    1a4b.c580.7000   Gi0/6
    300    0a1b.5c80.7000   Gi0/7

Restriction: All tasks must be performed using only covered topics.


Task 7.3a
~~~~~~~~~~~~

Make a copy of script from task 7.3.

Complete script: Sort output by VLAN number.

The result should be like this:

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


Note, vlan 1000 should be the last to be displayed. The correct sort can be achieved if vlan is a number rather than a string.

Restriction: All tasks must be performed using only covered topics.


Task 7.3b
~~~~~~~~~~~~

Make a copy of script from task 7.3a.

Redo script:

* Ask user to enter VLAN number.
* Display information only for specified VLAN.

Restriction: All tasks must be performed using only covered topics.

