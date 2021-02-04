WHERE
~~~~~

WHERE operator is used to specify a query. With the help of this operator it is possible to specify certain conditions under which data is selected. If condition is met the corresponding value is returned from table, if not - it is not returned.

Now there are only two enties in *switch* table:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str |
    +----------------+----------+------------+-------------------+
    2 rows in set
    Time: 0.033s

To create more entries in table you need to create more rows. Litecli has a **source** command that lets you upload SQL commands from a file.

File add_rows_to_testdb.txt is prepared to add entries:

.. code:: sql

    INSERT into switch values ('0030.A3AA.C1CC', 'sw3', 'Cisco 3750', 'London, Green Str');
    INSERT into switch values ('0040.A4AA.C2CC', 'sw4', 'Cisco 3850', 'London, Green Str');
    INSERT into switch values ('0050.A5AA.C3CC', 'sw5', 'Cisco 3850', 'London, Green Str');
    INSERT into switch values ('0060.A6AA.C4CC', 'sw6', 'C3750', 'London, Green Str');
    INSERT into switch values ('0070.A7AA.C5CC', 'sw7', 'Cisco 3650', 'London, Green Str');

To upload commands from a file you should execute the command:

::

    new_db.db> source add_rows_to_testdb.txt
    Query OK, 1 row affected
    Time: 0.023s

    Query OK, 1 row affected
    Time: 0.002s

    Query OK, 1 row affected
    Time: 0.003s

    Query OK, 1 row affected
    Time: 0.002s

    Query OK, 1 row affected
    Time: 0.002s

Now *switch* table looks like:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str |
    | 0070.A7AA.C5CC | sw7      | Cisco 3650 | London, Green Str |
    +----------------+----------+------------+-------------------+
    7 rows in set
    Time: 0.040s

Using the WHERE clause, you can show only those switches whose model is 3850:

.. code:: sql

    new_db.db> SELECT * from switch WHERE model = 'Cisco 3850';
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str |
    | 0040.A4AA.C2CC | sw4      | Cisco 3850 | London, Green Str |
    | 0050.A5AA.C3CC | sw5      | Cisco 3850 | London, Green Str |
    +----------------+----------+------------+-------------------+
    3 rows in set
    Time: 0.033s


WHERE operator allows you to specify more than a specific field value. If you add LIKE operator to it you can specify a field template.

Like with characters ``_`` and ``%`` indicates what the value should look like:

* ``_`` - denotes one character or number
* ``%`` - denotes zero, one or many characters

For example, if  *model* field is written in different formats the previous query will not be able to extract needed switches.

For example, for sw6 switch the model field is written in this format: C3750, but for sw1 and sw3 switches: Cisco 3750.

In this version, WHERE query does not show sw6:

.. code:: sql

    new_db.db> SELECT * from switch WHERE model = 'Cisco 3750';
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str |
    +----------------+----------+------------+-------------------+
    2 rows in set
    Time: 0.037s


If with WHERE operator use ``LIKE`` operator:

.. code:: sql

    new_db.db> SELECT * from switch WHERE model LIKE '%3750';
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str |
    | 0030.A3AA.C1CC | sw3      | Cisco 3750 | London, Green Str |
    | 0060.A6AA.C4CC | sw6      | C3750      | London, Green Str |
    +----------------+----------+------------+-------------------+
    3 rows in set
    Time: 0.040s

