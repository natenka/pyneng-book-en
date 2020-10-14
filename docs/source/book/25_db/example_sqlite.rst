SQLite use example
---------------------------

In section 15 there was an example of reviewing the output of command *show ip dhcp snooping binding*. In the output we received information about parameters of connected devices (interface, IP, MAC, VLAN).

In this variant you can only see all devices connected to switch. If you want to find out others based on one of the parameters, it’s not  convenient in this way.

For example, if you want to get information based on IP address about to which interface the host is connected, which MAC address it has and in which VLAN it is, then script is not very simple and more importantly, not convenient.

Let’s write information obtained from the output *sh ip dhcp snooping binding* to SQLite. This will allow do queries based on any parameter and get missing ones. For this example, it is sufficient to create a single table where information will be stored.

Table is defined in a separate dhcp_snooping_schema.sql file:

.. code:: sql

    create table if not exists dhcp (
        mac          text not NULL primary key,
        ip           text,
        vlan         text,
        interface    text
    );

For all fields the data type is "text".

MAC address is the primary key of our table which is logical because MAC address must be unique.

Additionally, by using expression ``create table if not exists`` -
SQLite will only create a table if it does not exist.

Now you have to create a database file, connect to database and create a table (create_sqlite_ver1.py file):

.. code:: python

    import sqlite3

    conn = sqlite3.connect('dhcp_snooping.db')

    print('Creating schema...')
    with open('dhcp_snooping_schema.sql', 'r') as f:
        schema = f.read()
        conn.executescript(schema)
    print("Done")

    conn.close()

Comments to file:

* during execution of ``conn = sqlite3.connect('dhcp_snooping.db')``: 

  * file dhcp_snooping.db is created if it does not exist
  * Connection object is created

* table is created in database (if it does not exist) based on commands specified in dhcp_snooping_schema.sql file:

  * dhcp_snooping_schema.sql file opens
  * ``schema = f.read()`` - whole file is read in one string
  * ``conn.executescript(schema)`` - executescript() method allows SQL to execute commands that are written in the file

Execution of script:

::

    $ python create_sqlite_ver1.py
    Creating schema...
    Done

The result should be a database file and a dhcp table.

You can check that table has been created with sqlite3 utility which allows you to execute queries directly in command line.

List of tables created is shown as follows:

::

    $ sqlite3 dhcp_snooping.db "SELECT name FROM sqlite_master WHERE type='table'"
    dhcp

Now it is necessary to write information from the output of *sh ip dhcp snooping binding* command to the table (dhcp_snooping.txt file):

::

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:09:BB:3D:D6:58   10.1.10.2        86250       dhcp-snooping   10    FastEthernet0/1
    00:04:A3:3E:5B:69   10.1.5.2         63951       dhcp-snooping   5     FastEthernet0/10
    00:05:B3:7E:9B:60   10.1.5.4         63253       dhcp-snooping   5     FastEthernet0/9
    00:09:BC:3F:A6:50   10.1.10.6        76260       dhcp-snooping   10    FastEthernet0/3
    Total number of bindings: 4

In the second version of the script, the output in dhcp_snooping.txt file is processed with regular expressions and then entries are added to database (create_sqlite_ver2.py file):

.. code:: python

    import sqlite3
    import re

    regex = re.compile('(\S+) +(\S+) +\d+ +\S+ +(\d+) +(\S+)')

    result = []

    with open('dhcp_snooping.txt') as data:
        for line in data:
            match = regex.search(line)
            if match:
                result.append(match.groups())

    conn = sqlite3.connect('dhcp_snooping.db')

    print('Creating schema...')
    with open('dhcp_snooping_schema.sql', 'r') as f:
        schema = f.read()
        conn.executescript(schema)
    print('Done')

    print('Inserting DHCP Snooping data')

    for row in result:
        try:
            with conn:
                query = '''insert into dhcp (mac, ip, vlan, interface)
                           values (?, ?, ?, ?)'''
                conn.execute(query, row)
        except sqlite3.IntegrityError as e:
            print('Error occured: ', e)

    conn.close()

.. note::

    For now, you should delete database file every time because script tries to create it every time you start.

Comments to the script:

* in regular expression that processes the output of *sh ip dhcp snooping binding*, numbered groups are used instead of named groups as it was in example of section `https://pyneng.readthedocs.io/en/latest/book/14_regex/4a_group_example.html>`__

  * groups were created only for those elements we are interested in

* result - a list that stores the result of processing the command output

  * but now there is no dictionaries but tuples with results 
  * this is necessary to enable them to be immediately written to  database

* Scroll elements in received list of tuples
* This script uses another version of database entry

  * *query* string describes a query. But instead of values, question marks are given. This query type allows dynamicly substite field values.
  * then execute() method is passed a query string and *row* tuple where values are

Execute the script:

::

    $ python create_sqlite_ver2.py
    Creating schema...
    Done
    Inserting DHCP Snooping data

Let’s check if data has been written:

::

    $ sqlite3 dhcp_snooping.db "select * from dhcp"
    -- Loading resources from /home/vagrant/.sqliterc

    mac                ip          vlan        interface
    -----------------  ----------  ----------  ---------------
    00:09:BB:3D:D6:58  10.1.10.2   10          FastEthernet0/1
    00:04:A3:3E:5B:69  10.1.5.2    5           FastEthernet0/1
    00:05:B3:7E:9B:60  10.1.5.4    5           FastEthernet0/9
    00:09:BC:3F:A6:50  10.1.10.6   10          FastEthernet0/3

Now let’s try to ask by a certain parameter:

::

    $ sqlite3 dhcp_snooping.db "select * from dhcp where ip = '10.1.5.2'"
    -- Loading resources from /home/vagrant/.sqliterc

    mac                ip          vlan        interface
    -----------------  ----------  ----------  ----------------
    00:04:A3:3E:5B:69  10.1.5.2    5           FastEthernet0/10

That is, it is now possible to get others parameters based on one parameter.

Let’s modify the script to make it check for the presence of dhcp_snooping.db. If you have a database file you don’t need to create a table, we believe it has already been created.

File create_sqlite_ver3.py:

.. code:: python

    import os
    import sqlite3
    import re

    data_filename = 'dhcp_snooping.txt'
    db_filename = 'dhcp_snooping.db'
    schema_filename = 'dhcp_snooping_schema.sql'

    regex = re.compile('(\S+) +(\S+) +\d+ +\S+ +(\d+) +(\S+)')

    result = []

    with open('dhcp_snooping.txt') as data:
        for line in data:
            match = regex.search(line)
            if match:
                result.append(match.groups())

    db_exists = os.path.exists(db_filename)

    conn = sqlite3.connect(db_filename)

    if not db_exists:
        print('Creating schema...')
        with open(schema_filename, 'r') as f:
            schema = f.read()
        conn.executescript(schema)
        print('Done')
    else:
        print('Database exists, assume dhcp table does, too.')

    print('Inserting DHCP Snooping data')

    for row in result:
        try:
            with conn:
                query = '''insert into dhcp (mac, ip, vlan, interface)
                           values (?, ?, ?, ?)'''
                conn.execute(query, row)
        except sqlite3.IntegrityError as e:
            print('Error occured: ', e)

    conn.close()

Now there is a verification of the presence of database file and dhcp_snooping.db file will only be created if it does not exist. Data is also written only if dhcp_snooping.db file is not created.

.. note::

    Separating the process of creating a table and completing it with the data is specified in tasks to the section.

If no file (delete it first):

::

    $ rm dhcp_snooping.db
    $ python create_sqlite_ver3.py
    Creating schema...
    Done
    Inserting DHCP Snooping data

Let’s check. In case the file already exists but the data is not written:

::

    $ rm dhcp_snooping.db

    $ python create_sqlite_ver1.py
    Creating schema...
    Done
    $ python create_sqlite_ver3.py
    Database exists, assume dhcp table does, too.
    Inserting DHCP Snooping data

If both DB and data are exist:

::

    $ python create_sqlite_ver3.py
    Database exists, assume dhcp table does, too.
    Inserting DHCP Snooping data
    Error occurred:  UNIQUE constraint failed: dhcp.mac
    Error occurred:  UNIQUE constraint failed: dhcp.mac
    Error occurred:  UNIQUE constraint failed: dhcp.mac
    Error occurred:  UNIQUE constraint failed: dhcp.mac

Now we make a separate script that deals with sending queries to database and displaying results. It should:

* expect parameters from user:

  * parameter name
  * parameter value

* provide normal output on request

File get_data_ver1.py:

.. code:: python

    import sqlite3
    import sys

    db_filename = 'dhcp_snooping.db'

    key, value = sys.argv[1:]
    keys = ['mac', 'ip', 'vlan', 'interface']
    keys.remove(key)

    conn = sqlite3.connect(db_filename)

    #Allows to further access data in columns by column name
    conn.row_factory = sqlite3.Row

    print('\nDetailed information for host(s) with', key, value)
    print('-' * 40)

    query = 'select * from dhcp where {} = ?'.format(key)
    result = conn.execute(query, (value, ))

    for row in result:
        for k in keys:
            print('{:12}: {}'.format(k, row[k]))
        print('-' * 40)

Comments to the script:

* key, value are read from arguments that passed to script

  * selected key is removed from *keys* list. Thus, only parameters that you want to display are left in the list

* connecting to DB

  * ``conn.row_factory = sqlite3.Row`` - allows further access data in column based on column names

* Select rows from database where key is equal to specified value

  * in SQL the values can be set by a question mark but you cannot give a column name. Therefore, the column name is substituted by row formatting and the value by SQL tool.
  * Pay attention to ``(value,)`` - tuple with one element is passed

* The resulting information is displayed to standard output stream: 

  * iterate over the results obtained and display only those fields that are in *keys* list

Let’s check the script.

Show host parameters with IP 10.1.10.2:

::

    $ python get_data_ver1.py ip 10.1.10.2

    Detailed information for host(s) with ip 10.1.10.2
    ----------------------------------------
    mac         : 00:09:BB:3D:D6:58
    vlan        : 10
    interface   : FastEthernet0/1
    ----------------------------------------

Show hosts in VLAN 10:

::

    $ python get_data_ver1.py vlan 10

    Detailed information for host(s) with vlan 10
    ----------------------------------------
    mac         : 00:09:BB:3D:D6:58
    ip          : 10.1.10.2
    interface   : FastEthernet0/1
    ----------------------------------------
    mac         : 00:07:BC:3F:A6:50
    ip          : 10.1.10.6
    interface   : FastEthernet0/3
    ----------------------------------------

The second version of the script to obtain data with minor improvements:

* Instead of rows formatting, a dictionary that describes queries corresponding to each key is used. 
* Checking the key that was selected
* Method keys() is used to obtain all columns that match the query

File get_data_ver2.py:

.. code:: python

    import sqlite3
    import sys

    db_filename = 'dhcp_snooping.db'

    query_dict = {
        'vlan': 'select mac, ip, interface from dhcp where vlan = ?',
        'mac': 'select vlan, ip, interface from dhcp where mac = ?',
        'ip': 'select vlan, mac, interface from dhcp where ip = ?',
        'interface': 'select vlan, mac, ip from dhcp where interface = ?'
    }

    key, value = sys.argv[1:]
    keys = query_dict.keys()

    if not key in keys:
        print('Enter key from {}'.format(', '.join(keys)))
    else:
        conn = sqlite3.connect(db_filename)
        conn.row_factory = sqlite3.Row

        print('\nDetailed information for host(s) with', key, value)
        print('-' * 40)

        query = query_dict[key]
        result = conn.execute(query, (value, ))

        for row in result:
            for row_name in row.keys():
                print('{:12}: {}'.format(row_name, row[row_name]))
            print('-' * 40)

There are several drawbacks to this script:

* does not check number of arguments that are passed to the script
* It would be good to collect information from different switches. To do this, you should add a field that indicates on which switch the entry was found

In addition, a lot of work needs to be done in the script that creates database and writes the data.

All improvements will be done in tasks of this section.
