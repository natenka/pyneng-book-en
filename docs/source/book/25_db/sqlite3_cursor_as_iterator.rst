Cursor as iterator
-------------------

If you want to process the resulting strings, use cursor as an iterator. It is not necessary to use fetch methods.

If you use execute() methods, the cursor is returned. Since cursor can be used as an iterator you can use it, for example, in **for** loop:

.. code:: python

    In [34]: result = cursor.execute('select * from switch')

    In [35]: for row in result:
        ...:     print(row)
        ...:
    ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str')
    ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str')
    ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str')
    ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')

The same option will work without assigning a variable:

.. code:: python

    In [36]: for row in cursor.execute('select * from switch'):
        ...:     print(row)
        ...:
    ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str')
    ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str')
    ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str')
    ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str')
    ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')

