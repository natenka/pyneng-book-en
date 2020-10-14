Using sqlite3 module without explicit cursor creation 
--------------------------------------------------------

Execute methods are available in Connection object and in Cursor object but fetch() methods are only available in Cursor object.

When using execute() methods with Connection object, cursor is returned as a result of execute() method. It can be used as an iterator and receive data without fetch() methods. This allows you not to create cursor when working with sqlite3 module.

Example of the resulting script (create_sw_inventory_ver1.py):

.. code:: python

    import sqlite3


    data = [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
            ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
            ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
            ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]


    con = sqlite3.connect('sw_inventory2.db')

    con.execute('''create table switch
                (mac text not NULL primary key, hostname text, model text, location text)'''
                )

    query = 'INSERT into switch values (?, ?, ?, ?)'
    con.executemany(query, data)
    con.commit()

    for row in con.execute('select * from switch'):
        print(row)

    con.close()

The result will be:

::

    $ python create_sw_inventory_ver1.py
    ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str')
    ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str')
    ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str')
    ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')

