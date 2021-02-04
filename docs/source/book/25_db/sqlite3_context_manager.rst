Connection as context manager
---------------------------------

After operations are completed the changes must be saved (apply ``commit``),
and then you can close connection if it is no longer needed.

Python allows you to use Connection object as a context manager.
In that case, you don't have to explicitly commit.

At the same time:

* If an exception occurs the transaction automatically rolls back
* if no exception, commit applies automatically

Example of using a database connection as a context manager
(create_sw_inventory_ver2.py):


.. code:: python

    import sqlite3


    data = [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
            ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
            ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
            ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]


    con = sqlite3.connect('sw_inventory3.db')
    con.execute('''create table switch
                   (mac text not NULL primary key, hostname text, model text, location text)'''
                )

    try:
        with con:
            query = 'INSERT into switch values (?, ?, ?, ?)'
            con.executemany(query, data)

    except sqlite3.IntegrityError as e:
        print('Error occured: ', e)

    for row in con.execute('select * from switch'):
        print(row)

    con.close()

Note that although a transaction will be rolled back when an exception occurs,
the exception itself must still be intercepted.

To check this functionality you should write to the table the data in which MAC
address is repeated. But before, in order to not repeat parts of the code, it
is better to split the code by functions in create_sw_inventory_ver2.py file:

.. code:: python

    from pprint import pprint
    import sqlite3


    data = [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
            ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
            ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
            ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]


    def create_connection(db_name):
        '''
        Function creates a connection to db_name database and returns it
        '''
        connection = sqlite3.connect(db_name)
        return connection


    def write_data_to_db(connection, query, data):
        '''
        Function awaits arguments:
         * connection - connection to DB
         * query - query to execute
         * data - data to be passed as a list of tuples

        Function attempts to write all data from *data* list.
        If data is saved successfully, changes are saved to database and returns True.

        If an error occurs during the writing process, transaction rolls back and function returns False.

        '''
        try:
            with connection:
                connection.executemany(query, data)
        except sqlite3.IntegrityError as e:
            print('Error occured: ', e)
            return False
        else:
            print('Data writing was successful')
            return True


    def get_all_from_db(connection, query):
        '''
        Function awaits arguments:
         * connection - connection to DB
         * query - query to execute

        Function returns data from database
        '''
        result = [row for row in connection.execute(query)]
        return result


    if __name__ == '__main__':
        con = create_connection('sw_inventory3.db')

        print('DB creation...')
        schema = '''create table switch
                    (mac text primary key, hostname text, model text, location text)'''
        con.execute(schema)

        query_insert = 'INSERT into switch values (?, ?, ?, ?)'
        query_get_all = 'SELECT * from switch'

        print('Write data to DB:')
        pprint(data)
        write_data_to_db(con, query_insert, data)
        print('\nChecking DB content')
        pprint(get_all_from_db(con, query_get_all))

        con.close()


The result of script execution is:

::

    $ python create_sw_inventory_ver2_functions.py
    Table creation...
    Data writing to DB:
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
    Data writing was successful

    Checking DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]

Now let's check how ``write_data_to_db`` function will work when there are
identical MAC addresses in the data.

File create_sw_inventory_ver3.py uses functions from
create_sw_inventory_ver2_functions.py file and implies that the script
will run after the previous data is written:

.. code:: python

    from pprint import pprint
    import sqlite3
    import create_sw_inventory_ver2_functions as dbf

    #MAC address of sw7 matches sw3 switch MAC in *data* list
    data2 = [('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
             ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
             ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960',
              'London, Green Str'), ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750',
                                     'London, Green Str')]

    con = dbf.create_connection('sw_inventory3.db')

    query_insert = "INSERT into switch values (?, ?, ?, ?)"
    query_get_all = "SELECT * from switch"

    print("\nChecking current content of DB")
    pprint(dbf.get_all_from_db(con, query_get_all))

    print('-' * 60)
    print("Trying to write data with a duplicate MAC address:")
    pprint(data2)
    dbf.write_data_to_db(con, query_insert, data2)
    print("\nChecking DB content")
    pprint(dbf.get_all_from_db(con, query_get_all))

    con.close()


In data2 list, sw7 switch has the same MAC address as sw3 switch
already existing in database.

Result of script execution:

::

    $ python create_sw_inventory_ver3.py

    Cheking current DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
    ------------------------------------------------------------
    Attempt to write data with repeating MAC address:
    [('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960', 'London, Green Str'),
     ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]
    Error occurred:  UNIQUE constraint failed: switch.mac

    Cheking DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]

Note that the content of switch table before and after adding of information
is the same. This means that no line from data2 list has been written.
This is because ``executemany`` method is used and within the same transaction
we try to write all four lines. If an error occurs with one of them,
all changes are reversed.

Sometimes it's exactly the kind of behavior you need. If you want to ignore
only row with errors you should use ``execute`` method and write each row separately.

File create_sw_inventory_ver4.py has ``write_rows_to_db`` function which writes data
in turn and if there is an error, only changes for specific data are rolled back:

.. code:: python

    from pprint import pprint
    import sqlite3
    import create_sw_inventory_ver2_functions as dbf

    #MAC address of sw7 matches sw3 switch MAC in *data* list
    data2 = [('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
             ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
             ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960', 'London, Green Str'),
             ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]


    def write_rows_to_db(connection, query, data, verbose=False):
        '''
        Function awaits arguments:
         * connection - connection to DB
         * query - query to execute
         * data - data to be passed as a list of tuples

        Function attempts to write a tuples in turn from *data* list.
        If tuple can be written successfully, changes are saved to database.
        If an error occurs while writing the tuple, transaction rolls back.


        Flag *verbose* controls whether messages about successful or unsuccessful tuple
        writing attempt.

        '''
        for row in data:
            try:
                with connection:
                    connection.execute(query, row)
            except sqlite3.IntegrityError as e:
                if verbose:
                    print("Error occurred while writing data '{}'".format(', '.join(row), e))
            else:
                if verbose:
                    print("Data writing  was successful '{}'".format(
                        ', '.join(row)))


    con = dbf.create_connection('sw_inventory3.db')

    query_insert = 'INSERT into switch values (?, ?, ?, ?)'
    query_get_all = 'SELECT * from switch'

    print('\nChecking current content of DB')
    pprint(dbf.get_all_from_db(con, query_get_all))

    print('-' * 60)
    print('Trying to write data with a duplicate MAC address:')
    pprint(data2)
    write_rows_to_db(con, query_insert, data2, verbose=True)
    print('\nChecking DB content')
    pprint(dbf.get_all_from_db(con, query_get_all))

    con.close()


The execution result is (missing only sw7):

::

    $ python create_sw_inventory_ver4.py

    Cheking current DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
    ------------------------------------------------------------
    Attempt to write data with repeating MAC address:
    [('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960', 'London, Green Str'),
     ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]
    Data "0055.AAAA.CCCC, sw5, Cisco 3750, London, Green Str" writing was successful
    Data "0066.BBBB.CCCC, sw6, Cisco 3780, London, Green Str" writing was successful
    While writing data "0000.AAAA.DDDD, sw7, Cisco 2960, London, Green Str" the error occured
    Data "0088.AAAA.CCCC, sw8, Cisco 3750, London, Green Str" writing was successful

    Cheking DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str'),
     ('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
     ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]

