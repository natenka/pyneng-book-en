REPLACE
~~~~~~~

REPLACE operator is used to add or replace data in the table.

.. note::

    REPLACE operator may not be supported in all DBMS.

When a field uniqueness condition is violated, an expression with REPLACE operator:

* deletes the existing string that caused the violation
* adds a new line

An example of uniqueness condition rule violation:

.. code:: sql

    new_db.db> INSERT INTO switch VALUES ('0030.A3AA.C1CC', 'sw3', 'Cisco 3850', 'London, Green Str', '10.255.1.3', 255);
    UNIQUE constraint failed: switch.mac


There are two types of REPLACE expression:

.. code:: sql

    new_db.db> INSERT OR REPLACE INTO switch VALUES ('0030.A3AA.C1CC', 'sw3', 'Cisco 3850', 'London, Green Str', '10.255.1.3', 255);
    Query OK, 1 row affected
    Time: 0.010s

Or a shorter version:

.. code:: sql

    new_db.db> REPLACE INTO switch VALUES ('0030.A3AA.C1CC', 'sw3', 'Cisco 3850', 'London, Green Str', '10.255.1.3', 255);
    Query OK, 1 row affected
    Time: 0.009s

The result of any of these commands is to replace sw3 switch model:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.D1DD.E1EE | sw1      | Cisco 3850 | London, Green Str | 10.255.1.1 | 255       |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | 10.255.1.2 | 255       |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | 10.255.1.4 | 255       |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | 10.255.1.5 | 255       |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | 10.255.1.6 | 255       |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | 10.255.1.7 | 255       |
    | 0030.A3AA.C1CC | sw3      | Cisco 3850 | London, Green Str | 10.255.1.3 | 255       |
    +----------------+----------+------------+-------------------+------------+-----------+

In this case, MAC address in new entry is the same as in existing one, so the replacement occurs.

.. note::

    If not all fields have been specified, the new entry will contain only those fields that have been specified. This is because REPLACE first removes an existing entry.

For entry which was added without uniqueness violation, REPLACE functions as a normal INSERT:

.. code:: sql

    new_db.db> REPLACE INTO switch VALUES ('0080.A8AA.C8CC', 'sw8', 'Cisco 3850', 'London, Green Str', '10.255.1.8', 255);
    Query OK, 1 row affected
    Time: 0.009s

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.D1DD.E1EE | sw1      | Cisco 3850 | London, Green Str | 10.255.1.1 | 255       |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | 10.255.1.2 | 255       |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | 10.255.1.4 | 255       |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | 10.255.1.5 | 255       |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | 10.255.1.6 | 255       |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | 10.255.1.7 | 255       |
    | 0030.A3AA.C1CC | sw3      | Cisco 3850 | London, Green Str | 10.255.1.3 | 255       |
    | 0080.A8AA.C8CC | sw8      | Cisco 3850 | London, Green Str | 10.255.1.8 | 255       |
    +----------------+----------+------------+-------------------+------------+-----------+
    8 rows in set
    Time: 0.034s
