UPDATE
~~~~~~

UPDATE operator is used to change an existing table entry.

Usually, UPDATE is used with WHERE operator to specify which entry to change.

With UPDATE you can fill in new columns in the table.

For example, add an IP address for sw1 switch:

.. code:: sql

    new_db.db> UPDATE switch set mngmt_ip = '10.255.1.1' WHERE hostname = 'sw1';
    Query OK, 1 row affected
    Time: 0.009s

Now the table is like this:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str | 10.255.1.1 | <null>    |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str | <null>     | <null>    |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | <null>     | <null>    |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | <null>     | <null>    |
    +----------------+----------+------------+-------------------+------------+-----------+
    7 rows in set
    Time: 0.035s

VLAN number can be changed in the same way:

.. code:: sql

    new_db.db> UPDATE switch set mngmt_vid = 255 WHERE hostname = 'sw1';
    Query OK, 1 row affected
    Time: 0.009s

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str | 10.255.1.1 | 255       |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str | <null>     | <null>    |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | <null>     | <null>    |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | <null>     | <null>    |
    +----------------+----------+------------+-------------------+------------+-----------+
    7 rows in set
    Time: 0.037s


You can change several fields at a time:

.. code:: sql

    new_db.db> UPDATE switch set mngmt_ip = '10.255.1.2', mngmt_vid = 255 WHERE hostname = 'sw2'
    Query OK, 1 row affected
    Time: 0.009s

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str | 10.255.1.1 | 255       |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | 10.255.1.2 | 255       |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str | <null>     | <null>    |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | <null>     | <null>    |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | <null>     | <null>    |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | <null>     | <null>    |
    +----------------+----------+------------+-------------------+------------+-----------+
    7 rows in set
    Time: 0.033s


To avoid filling fields mngmt_ip and mngmt_vid manually, fill in the rest from the update_fields_in_testdb.txt file (command ``source update_fields_in_testdb.txt``):

::

    UPDATE switch set mngmt_ip = '10.255.1.3', mngmt_vid = 255 WHERE hostname = 'sw3';
    UPDATE switch set mngmt_ip = '10.255.1.4', mngmt_vid = 255 WHERE hostname = 'sw4';
    UPDATE switch set mngmt_ip = '10.255.1.5', mngmt_vid = 255 WHERE hostname = 'sw5';
    UPDATE switch set mngmt_ip = '10.255.1.6', mngmt_vid = 255 WHERE hostname = 'sw6';
    UPDATE switch set mngmt_ip = '10.255.1.7', mngmt_vid = 255 WHERE hostname = 'sw7';

After commands upload the table is as follows:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str | 10.255.1.1 | 255       |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | 10.255.1.2 | 255       |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str | 10.255.1.3 | 255       |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | 10.255.1.4 | 255       |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | 10.255.1.5 | 255       |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | 10.255.1.6 | 255       |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | 10.255.1.7 | 255       |
    +----------------+----------+------------+-------------------+------------+-----------+
    7 rows in set
    Time: 0.038s

Now suppose that sw1 was replaced from 3750 model to 3850. Accordingly, not only model field but also MAC address field was changed.

Making changes:

.. code:: sql

    new_db.db> UPDATE switch set model = 'Cisco 3850', mac = '0010.D1DD.E1EE' WHERE hostname = 'sw1';
    Query OK, 1 row affected
    Time: 0.009s

The result will be:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+------------+-----------+
    | mac            | hostname | model      | location          | mngmt_ip   | mngmt_vid |
    +----------------+----------+------------+-------------------+------------+-----------+
    | 0010.D1DD.E1EE | sw1      | Cisco 3850 | London, Green Str | 10.255.1.1 | 255       |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str | 10.255.1.2 | 255       |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str | 10.255.1.3 | 255       |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str | 10.255.1.4 | 255       |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str | 10.255.1.5 | 255       |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str | 10.255.1.6 | 255       |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str | 10.255.1.7 | 255       |
    +----------------+----------+------------+-------------------+------------+-----------+
    7 rows in set
    Time: 0.049s

