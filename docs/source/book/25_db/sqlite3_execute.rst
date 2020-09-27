Executing SQL commands
----------------------

There are several methods for execution of SQL commands in module:

* ``execute()`` - method for executing one SQL expression 
* ``executemany()`` - method allows to execute one SQL expression for a sequence of parameters (or for iterator) 
* ``executescript()`` - method allows to execute multiple SQL expressions at once

Method execute
^^^^^^^^^^^^^

Method execute() allows one SQL command to be executed.

First, create connection and cursor:

.. code:: python

    In [1]: import sqlite3

    In [2]: connection = sqlite3.connect('sw_inventory.db')

    In [3]: cursor = connection.cursor()

Creates a *switch* table using execute():

.. code:: python

    In [4]: cursor.execute("create table switch (mac text not NULL primary key, hostname text, model text, location text)")
    Out[4]: <sqlite3.Cursor at 0x1085be880>

SQL expressions can be parameterized - data can be substituted by special values. Due to this you can use the same SQL command to transfer different data.

For example, *switch* table needs to be filled with data from *data* list:

.. code:: python

    In [5]: data = [
       ...: ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
       ...: ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
       ...: ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
       ...: ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]

You can use this query:

.. code:: python

    In [6]: query = "INSERT into switch values (?, ?, ?, ?)"

The question marks in command are used to fill in the data that will be passed to execute().

Data can now be passed as follows:

.. code:: python

    In [7]: for row in data:
       ...:     cursor.execute(query, row)
       ...:

The second argument that is passed to execute() must be a tuple. If you want to transfer a tuple with one element, ``(value, )`` entry is used.

For changes to be applied, commit must be executed (note that commit() method is called at the connection):

.. code:: python

    In [8]: connection.commit()

Now, when querying from sqlite3 command line you can see these rows in *switch* table:	

::

    $ litecli sw_inventory.db
    Version: 1.0.0
    Mail: https://groups.google.com/forum/#!forum/litecli-users
    Github: https://github.com/dbcli/litecli
    sw_inventory.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0000.AAAA.CCCC | sw1      | Cisco 3750 | London, Green Str |
    | 0000.BBBB.CCCC | sw2      | Cisco 3780 | London, Green Str |
    | 0000.AAAA.DDDD | sw3      | Cisco 2960 | London, Green Str |
    | 0011.AAAA.CCCC | sw4      | Cisco 3750 | London, Green Str |
    +----------------+----------+------------+-------------------+
    4 rows in set
    Time: 0.039s
    sw_inventory.db>


Method executemany
^^^^^^^^^^^^^^^^^

Method executemany() allows one SQL command to be executed for parameter sequence (or for iterator).

Using executemany() method you can add a similar data list to *switch* table by a single command.

For example, you should add data from the *data2* list to *switch* table:

.. code:: python

    In [9]: data2 = [
       ...: ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str'),
       ...: ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str'),
       ...: ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str'),
       ...: ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')]

To do this, use a similar request:

.. code:: python

    In [10]: query = "INSERT into switch values (?, ?, ?, ?)"

Now you can pass data to executemany():

.. code:: python

    In [11]: cursor.executemany(query, data2)
    Out[11]: <sqlite3.Cursor at 0x10ee5e810>

    In [12]: connection.commit()

After commit, data is available in the table:

::

    $ litecli sw_inventory.db
    Version: 1.0.0
    Mail: https://groups.google.com/forum/#!forum/litecli-users
    Github: https://github.com/dbcli/litecli
    sw_inventory.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0000.AAAA.CCCC | sw1      | Cisco 3750 | London, Green Str |
    | 0000.BBBB.CCCC | sw2      | Cisco 3780 | London, Green Str |
    | 0000.AAAA.DDDD | sw3      | Cisco 2960 | London, Green Str |
    | 0011.AAAA.CCCC | sw4      | Cisco 3750 | London, Green Str |
    | 0000.1111.0001 | sw5      | Cisco 3750 | London, Green Str |
    | 0000.1111.0002 | sw6      | Cisco 3750 | London, Green Str |
    | 0000.1111.0003 | sw7      | Cisco 3750 | London, Green Str |
    | 0000.1111.0004 | sw8      | Cisco 3750 | London, Green Str |
    +----------------+----------+------------+-------------------+
    8 rows in set
    Time: 0.034s

Method executemany() placed corresponding tuples to SQL command and all data was added to the table.

Method executescript
^^^^^^^^^^^^^^^^^^^

Method executescript allows multiple SQL expressions to be executed at once.

This method is particularly useful when creating tables:

.. code:: python

    In [13]: connection = sqlite3.connect('new_db.db')

    In [14]: cursor = connection.cursor()

    In [15]: cursor.executescript('''
        ...:     create table switches(
        ...:         hostname     text not NULL primary key,
        ...:         location     text
        ...:     );
        ...:
        ...:     create table dhcp(
        ...:         mac          text not NULL primary key,
        ...:         ip           text,
        ...:         vlan         text,
        ...:         interface    text,
        ...:         switch       text not null references switches(hostname)
        ...:     );
        ...: ''')
    Out[15]: <sqlite3.Cursor at 0x10efd67a0>

