Fetching query results
-----------------------------

There are several ways to get query results in sqlite3:

* using ``fetch...()`` - depending on the method one, more or all rows are returned
* using cursor as an iterator - iterator returns

Method fetchone
^^^^^^^^^^^^^^

Method fetchone() returns one data row.

Example of fetching information from sw_inventory.db database:

.. code:: python

    In [16]: import sqlite3

    In [17]: connection = sqlite3.connect('sw_inventory.db')

    In [18]: cursor = connection.cursor()

    In [19]: cursor.execute('select * from switch')
    Out[19]: <sqlite3.Cursor at 0x104eda810>

    In [20]: cursor.fetchone()
    Out[20]: ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str')

Note that while the SQL query requests all table content, fetchone() returned only one row.

If you re-call method, it returns the next row:

.. code:: python

    In [21]: print(cursor.fetchone())
    ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str')

Similarly, method will return the next rows. After processing all rows, method starts returning None.

In this way, method can be used in the loop, for example:

.. code:: python

    In [22]: cursor.execute('select * from switch')
    Out[22]: <sqlite3.Cursor at 0x104eda810>

    In [23]: while True:
       ...:     next_row = cursor.fetchone()
       ...:     if next_row:
       ...:         print(next_row)
       ...:     else:
       ...:         break
       ...:
    ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str')
    ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str')
    ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str')
    ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')

Method fetchmany
^^^^^^^^^^^^^^^

Method fetchmany() returns a list of data rows.

Method syntax:

.. code:: python

    cursor.fetchmany([size=cursor.arraysize])

Size parameter allows you to specify how many rows are returned. By default the size parameter is cursor.arraysize:

.. code:: python

    In [24]: print(cursor.arraysize)
    1

For example, you can return three rows at a time from query:

.. code:: python


    In [25]: cursor.execute('select * from switch')
    Out[25]: <sqlite3.Cursor at 0x104eda810>

    In [26]: from pprint import pprint

    In [27]: while True:
        ...:     three_rows = cursor.fetchmany(3)
        ...:     if three_rows:
        ...:         pprint(three_rows)
        ...:     else:
        ...:         break
        ...:
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str')]
    [('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str')]
    [('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')]

Method displays required number of rows and if amount of rows are less than the size parameter, it returns remaining rows.

Method fetchall
^^^^^^^^^^^^^^

Method fetchall() returns all rows as a list:

.. code:: python

    In [28]: cursor.execute('select * from switch')
    Out[28]: <sqlite3.Cursor at 0x104eda810>

    In [29]: cursor.fetchall()
    Out[29]:
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')]

An important aspect of method - it returns all remaining rows.

That is, if fetchone() method was used before fetchall(), then fetchall() would return remaining query rows:

.. code:: python

    In [30]: cursor.execute('select * from switch')
    Out[30]: <sqlite3.Cursor at 0x104eda810>

    In [31]: cursor.fetchone()
    Out[31]: ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str')

    In [32]: cursor.fetchone()
    Out[32]: ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str')

    In [33]: cursor.fetchall()
    Out[33]:
    [('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str'),
     ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')]

Method fetchmany() works similarly in this aspect.

