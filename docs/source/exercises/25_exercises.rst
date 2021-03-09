.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 25.1
~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

You need to create two scripts:

1. create_db.py
2. add_data.py

The code in scripts should be broken down into functions.
It is up to you to decide which functions and how to split the code.
Some of the code can be global.

create_db.py - this script should contain the functionality for creating a database:

* check for the presence of a database file
* if the file does not exist, according to the description of the database schema
  in the dhcp_snooping_schema.sql file, the database must be created
* the name of the database file is dhcp_snooping.db

The database should contain two tables (the schema is described
in the dhcp_snooping_schema.sql file):

* switches - it contains data about switches
* dhcp - information obtained from the output
  of sh ip dhcp snooping binding is stored here

An example of script execution when there is no dhcp_snooping.db file:

::

    $ python create_db.py
    Database creation...

After creating the file:

::

    $ python create_db.py
    Database exists

add_data.py script adds data to the database.
This script should add data from sh ip dhcp snooping binding output
and switch information

Accordingly, there should be two parts in the add_data.py file:

* information about switches is added to the switches table.
  Switch data are in the switches.yml file
* information based on the output of sh ip dhcp snooping binding is added to the dhcp table

  * output from three switches in files: files sw1_dhcp_snooping.txt,
    sw2_dhcp_snooping.txt, sw3_dhcp_snooping.txt
  * since the dhcp table has changed, and now there is a switch field, it must
    also be filled. The switch name is derived from the data file name

An example of script execution when the database has not yet been created:

::

    $ python add_data.py
    The database does not exist. Before adding data, you need to create it

An example of executing the script for the first time after creating the database:

::

    $ python add_data.py
    Adding data to the switches table...
    Add data to dhcp table...

An example of script execution after the data has been added to the table
(the order of adding data can be arbitrary, but messages must
output similarly to the output below):

::

    $ python add_data.py
    Adding data to the switches table...
    While adding data: ('sw1', 'London, 21 New Globe Walk') An error occurred: UNIQUE constraint failed: switches.hostname
    While adding data: ('sw2', 'London, 21 New Globe Walk') An error occurred: UNIQUE constraint failed: switches.hostname
    While adding data: ('sw3', 'London, 21 New Globe Walk') An error occurred: UNIQUE constraint failed: switches.hostname
    Adding data to the dhcp table...
    While adding data: ('00:09:BB:3D:D6:58', '10.1.10.2', '10', 'FastEthernet0/1', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:04:A3:3E:5B:69', '10.1.5.2', '5', 'FastEthernet0/10', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:05:B3:7E:9B:60', '10.1.5.4', '5', 'FastEthernet0/9', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:07:BC:3F:A6:50', '10.1.10.6', '10', 'FastEthernet0/3', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:09:BC:3F:A6:50', '192.168.100.100', '1', 'FastEthernet0/7', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:E9:BC:3F:A6:50', '100.1.1.6', '3', 'FastEthernet0/20', 'sw3') An error occurred: UNIQUE constraint failed: dhcp.mac
    ...

At this stage, both scripts are called with no arguments.

The code in scripts should be broken down into functions.
It is up to you to decide which functions and how to split the code.
Some of the code can be global.


Task 25.2
~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

In this task, you need to create the get_data.py script.

The code in the script should be broken down into functions.
It is up to you to decide which functions and how to split the code.
Some of the code can be global.

The script can be passed arguments and, depending on the arguments,
you need to display different information.
If the script is called:

* with no arguments, print the entire contents of the dhcp table
* with two arguments, display information from the dhcp table that matches
  the field and value
* with any other number of arguments, print a message that the script
  only supports two or zero arguments

The database file can be copied from task 25.1.

Examples of output for different numbers and values of arguments:

::

    $ python get_data.py
    The dhcp table has the following entries:
    -----------------  ---------------  --  ----------------  ---
    00:09:BB:3D:D6:58  10.1.10.2        10  FastEthernet0/1   sw1
    00:04:A3:3E:5B:69  10.1.5.2          5  FastEthernet0/10  sw1
    00:05:B3:7E:9B:60  10.1.5.4          5  FastEthernet0/9   sw1
    00:07:BC:3F:A6:50  10.1.10.6        10  FastEthernet0/3   sw1
    00:09:BC:3F:A6:50  192.168.100.100   1  FastEthernet0/7   sw1
    00:E9:BC:3F:A6:50  100.1.1.6         3  FastEthernet0/20  sw3
    00:E9:22:11:A6:50  100.1.1.7         3  FastEthernet0/21  sw3
    00:A9:BB:3D:D6:58  10.1.10.20       10  FastEthernet0/7   sw2
    00:B4:A3:3E:5B:69  10.1.5.20         5  FastEthernet0/5   sw2
    00:C5:B3:7E:9B:60  10.1.5.40         5  FastEthernet0/9   sw2
    00:A9:BC:3F:A6:50  10.1.10.60       20  FastEthernet0/2   sw2
    -----------------  ---------------  --  ----------------  ---

    $ python get_data.py vlan 10

    Information about devices with the following parameters: vlan 10
    -----------------  ----------  --  ---------------  ---
    00:09:BB:3D:D6:58  10.1.10.2   10  FastEthernet0/1  sw1
    00:07:BC:3F:A6:50  10.1.10.6   10  FastEthernet0/3  sw1
    00:A9:BB:3D:D6:58  10.1.10.20  10  FastEthernet0/7  sw2
    -----------------  ----------  --  ---------------  ---

    $ python get_data.py ip 10.1.10.2

    Information about devices with the following parameters: ip 10.1.10.2
    -----------------  ---------  --  ---------------  ---
    00:09:BB:3D:D6:58  10.1.10.2  10  FastEthernet0/1  sw1
    -----------------  ---------  --  ---------------  ---

    $ python get_data.py vln 10
    This parameter is not supported.
    Valid parameter values: mac, ip, vlan, interface, switch

    $ python get_data.py ip vlan 10
    Please enter two or zero arguments

Task 25.3
~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

In previous tasks, information was added to an empty database.
In this task, the script should work correctly even in a situation where
the database already contains information.

Copy the add_data.py script from task 25.1 and try running it again
on the existing database. The output should be like this:

::

    $ python add_data.py
    Adding data to the switches table...
    While adding data: ('sw1', 'London, 21 New Globe Walk') An error occurred: UNIQUE constraint failed: switches.hostname
    While adding data: ('sw2', 'London, 21 New Globe Walk') An error occurred: UNIQUE constraint failed: switches.hostname
    While adding data: ('sw3', 'London, 21 New Globe Walk') An error occurred: UNIQUE constraint failed: switches.hostname
    Adding data to the dhcp table...
    While adding data: ('00:09:BB:3D:D6:58', '10.1.10.2', '10', 'FastEthernet0/1', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:04:A3:3E:5B:69', '10.1.5.2', '5', 'FastEthernet0/10', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:05:B3:7E:9B:60', '10.1.5.4', '5', 'FastEthernet0/9', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:07:BC:3F:A6:50', '10.1.10.6', '10', 'FastEthernet0/3', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    While adding data: ('00:09:BC:3F:A6:50', '192.168.100.100', '1', 'FastEthernet0/7', 'sw1') An error occurred: UNIQUE constraint failed: dhcp.mac
    ... (the command output is abbreviated)

When creating the database schema, it was explicitly stated that the MAC address
field must be unique. Therefore, when adding an entry with the same MAC address,
an exception (error) is raised. Task 25.1 handles the exception and writes a message
to stdout.

In this task, it is assumed that information is periodically read from
the switches and written to files. After that, the information from the files
must be transferred to the database. At the same time, there may be changes in the
new data: MAC disappeared, MAC switched to another port/vlan, a new MAC appeared, etc.


In this task, in the dhcp table, you need to create a new field, active, which
will indicate whether the record is up-to-date. The new database schema is
located in the dhcp_snooping_schema.sql file.

The active field can have the following values:

* 0 - means False. Used to mark an entry as inactive
* 1 - True. Used to indicate that a record is active

Every time information from files with DHCP snooping output is added again, all
existing entries (for this switch) must be marked as inactive (active = 0).
You can then update the information and mark the new records as active (active = 1).

Thus, the old records will remain in the database for MAC addresses that are
currently inactive, and updated information for active addresses will appear.


For example, the dhcp table contains the following entries:

::

    mac                ip          vlan        interface         switch      active
    -----------------  ----------  ----------  ----------------  ----------  ----------
    00:09:BB:3D:D6:58  10.1.10.2   10          FastEthernet0/1   sw1         1
    00:04:A3:3E:5B:69  10.1.5.2    5           FastEthernet0/10  sw1         1
    00:05:B3:7E:9B:60  10.1.5.4    5           FastEthernet0/9   sw1         1
    00:07:BC:3F:A6:50  10.1.10.6   10          FastEthernet0/3   sw1         1
    00:09:BC:3F:A6:50  192.168.10  1           FastEthernet0/7   sw1         1

And you need to add the following information from the file:

::

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:09:BB:3D:D6:58   10.1.10.2        86250       dhcp-snooping   10    FastEthernet0/1
    00:04:A3:3E:5B:69   10.1.15.2        63951       dhcp-snooping   15    FastEthernet0/15
    00:05:B3:7E:9B:60   10.1.5.4         63253       dhcp-snooping   5     FastEthernet0/9
    00:07:BC:3F:A6:50   10.1.10.6        76260       dhcp-snooping   10    FastEthernet0/5


After adding the data, the table should look like this:

::

    mac                ip               vlan        interface         switch      active
    -----------------  ---------------  ----------  ---------------   ----------  ----------
    00:09:BC:3F:A6:50  192.168.100.100  1           FastEthernet0/7   sw1         0
    00:09:BB:3D:D6:58  10.1.10.2        10          FastEthernet0/1   sw1         1
    00:04:A3:3E:5B:69  10.1.15.2        15          FastEthernet0/15  sw1         1
    00:05:B3:7E:9B:60  10.1.5.4         5           FastEthernet0/9   sw1         1
    00:07:BC:3F:A6:50  10.1.10.6        10          FastEthernet0/5   sw1         1


The new information should overwrite the previous one:

* MAC 00:04:A3:3E:5B:69 went to a different port and got into a different
  interface and got a different IP address
* MAC 00:07:BC:3F:A6:50 switched to another port

If some MAC address is not in the new file, it must be left in the database
with the value active = 0: MAC addresses 00:09:BC:3F:A6:50 not in new information
(turned off the computer)

Modify the add_data.py script so that the new conditions are met and the active
field is populated.

The code in the script should be broken down into functions. It is up to you to
decide which functions and how to split the code. Some of the code can be global.

To check task and operation of a new field, first add information to the database
from files sw*_dhcp_snooping.txt, and then add information from files
new_data/sw*_dhcp_snooping.txt.


The data should look like this (the lines can be in any order)

::

    -----------------  ---------------  --  ----------------  ---  -
    00:09:BC:3F:A6:50  192.168.100.100   1  FastEthernet0/7   sw1  0
    00:C5:B3:7E:9B:60  10.1.5.40         5  FastEthernet0/9   sw2  0
    00:09:BB:3D:D6:58  10.1.10.2        10  FastEthernet0/1   sw1  1
    00:04:A3:3E:5B:69  10.1.15.2        15  FastEthernet0/15  sw1  1
    00:05:B3:7E:9B:60  10.1.5.4          5  FastEthernet0/9   sw1  1
    00:07:BC:3F:A6:50  10.1.10.6        10  FastEthernet0/5   sw1  1
    00:E9:BC:3F:A6:50  100.1.1.6         3  FastEthernet0/20  sw3  1
    00:E9:22:11:A6:50  100.1.1.7         3  FastEthernet0/21  sw3  1
    00:A9:BB:3D:D6:58  10.1.10.20       10  FastEthernet0/7   sw2  1
    00:B4:A3:3E:5B:69  10.1.5.20         5  FastEthernet0/5   sw2  1
    00:A9:BC:3F:A6:50  10.1.10.65       20  FastEthernet0/2   sw2  1
    00:A9:33:44:A6:50  10.1.10.77       10  FastEthernet0/4   sw2  1
    -----------------  ---------------  --  ----------------  ---  -



Task 25.4
~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

Copy file get_data.py from task 25.2.
Add to the script support for the active column, which we added in task 25.3.

Now, when requesting information, active records should be displayed first,
and then, inactive ones. If there are no inactive records, do not display
the "Inactive records" header.

Examples of script execution:

::

    $ python get_data.py
    The dhcp table has the following entries:

    Active entries:
    -----------------  ----------  --  ----------------  ---  -
    00:09:BB:3D:D6:58  10.1.10.2   10  FastEthernet0/1   sw1  1
    00:04:A3:3E:5B:69  10.1.15.2   15  FastEthernet0/15  sw1  1
    00:05:B3:7E:9B:60  10.1.5.4     5  FastEthernet0/9   sw1  1
    00:07:BC:3F:A6:50  10.1.10.6   10  FastEthernet0/5   sw1  1
    00:E9:BC:3F:A6:50  100.1.1.6    3  FastEthernet0/20  sw3  1
    00:E9:22:11:A6:50  100.1.1.7    3  FastEthernet0/21  sw3  1
    00:A9:BB:3D:D6:58  10.1.10.20  10  FastEthernet0/7   sw2  1
    00:B4:A3:3E:5B:69  10.1.5.20    5  FastEthernet0/5   sw2  1
    00:A9:BC:3F:A6:50  10.1.10.65  20  FastEthernet0/2   sw2  1
    00:A9:33:44:A6:50  10.1.10.77  10  FastEthernet0/4   sw2  1
    -----------------  ----------  --  ----------------  ---  -

    Inactive entries:
    -----------------  ---------------  -  ---------------  ---  -
    00:09:BC:3F:A6:50  192.168.100.100  1  FastEthernet0/7  sw1  0
    00:C5:B3:7E:9B:60  10.1.5.40        5  FastEthernet0/9  sw2  0
    -----------------  ---------------  -  ---------------  ---  -

    $ python get_data.py vlan 5

    Information about devices with the following parameters: vlan 5

    Active entries:
    -----------------  ---------  -  ---------------  ---  -
    00:05:B3:7E:9B:60  10.1.5.4   5  FastEthernet0/9  sw1  1
    00:B4:A3:3E:5B:69  10.1.5.20  5  FastEthernet0/5  sw2  1
    -----------------  ---------  -  ---------------  ---  -

    Inactive entries:
    -----------------  ---------  -  ---------------  ---  -
    00:C5:B3:7E:9B:60  10.1.5.40  5  FastEthernet0/9  sw2  0
    -----------------  ---------  -  ---------------  ---  -


    $ python get_data.py vlan 10

    Information about devices with the following parameters: vlan 10

    Active entries:
    -----------------  ----------  --  ---------------  ---  -
    00:09:BB:3D:D6:58  10.1.10.2   10  FastEthernet0/1  sw1  1
    00:07:BC:3F:A6:50  10.1.10.6   10  FastEthernet0/5  sw1  1
    00:A9:BB:3D:D6:58  10.1.10.20  10  FastEthernet0/7  sw2  1
    00:A9:33:44:A6:50  10.1.10.77  10  FastEthernet0/4  sw2  1
    -----------------  ----------  --  ---------------  ---  -

Task 25.5
~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

After completing tasks 25.1 - 25.5, information about inactive records remains
in the database. And, if some MAC address did not appear in new records,
the record with it may remain in the database forever.

And while it can be useful to see where the MAC address was last located,
it is not very useful to keep this information permanently. Instead, you can
delete a record if, for example, it has been stored in the database for more
than a month.


In order to be able to delete records by date, you need to enter a new field
in which the last time the record was added will be recorded.

The new field is called last_active and must contain a string in the format:
YYYY-MM-DD HH:MM:SS.

In this task you need to:

* change the dhcp table accordingly and add a new field. The table can be
  changed from cli sqlite, but the dhcp_snooping_schema.sql file must also be changed
* modify the add_data.py script to add time to each entry

You can get a string with time and date in the specified format using
the datetime function in an SQL query. The syntax is as follows:

.. code:: sql

    sqlite> insert into dhcp (mac, ip, vlan, interface, switch, active, last_active)
       ...> values ('00:09:BC:3F:A6:50', '192.168.100.100', '1', 'FastEthernet0/7', 'sw1', '0', datetime('now'));

That is, instead of the value that is written to the database, you must
specify datetime('now').

After this command, the following entry will appear in the database:

::

    mac                ip               vlan  interface        switch  active  last_active
    -----------------  ---------------  ----  ---------------  ------  ------  -------------------
    00:09:BC:3F:A6:50  192.168.100.100  1     FastEthernet0/7  sw1     0       2021-03-09 07:46:31


Task 25.5a
~~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

After completing task 25.5, the dhcp table has a new last_active field.

Update the add_data.py script so that it removes all records that were active
more than 7 days ago.
In order to get such records, you can manually update the last_active field
in some records and set the time to 7 or more days.

The task file shows an example of working with objects of the datetime module.
Shows how to get the date 7 days ago. It will be necessary to compare
the last_active time with this date.

Please note that date strings that are written to the database can be compared
with each other.

.. code:: python

    from datetime import timedelta, datetime

    now = datetime.today().replace(microsecond=0)
    week_ago = now - timedelta(days=7)

    #print(now)
    #print(week_ago)
    #print(now > week_ago)
    #print(str(now) > str(week_ago))

Task 25.6
~~~~~~~~~~~~

There are no tests for the tasks of the 25th chapter!

This task contains the parse_dhcp_snooping.py file.
You cannot change anything in the parse_dhcp_snooping.py file.

The file creates several functions and describes the command line arguments
that the file takes.
There is support for arguments to perform all the actions that, in the previous
tasks, were performed in the files create_db.py, add_data.py and get_data.py.

The parse_dhcp_snooping.py file contains this line:
import parse_dhcp_snooping_functions as pds

And the goal of this task is to create all the necessary functions
in the parse_dhcp_snooping_functions.py file based on the information
in the parse_dhcp_snooping.py file.

From the parse_dhcp_snooping.py file, you need to decide:

* what functions should be in the parse_dhcp_snooping_functions.py file
* what parameters to create in these functions

It is necessary to create the corresponding functions and transfer to them
the functionality that is described in the previous tasks.
All the necessary information is present in the create, add, get functions,
in the parse_dhcp_snooping.py file.

In principle, to complete the task, it is not necessary to understand
the argparse module, but, you can read about it in `this section <https://pyneng.readthedocs.io/en/latest/book/additional_info/argparse.html>`__


To make it easier to get started, try creating the required functions
in parse_dhcp_snooping_functions.py and just print the function arguments.
Then, you can create functions that request information from the database
(the database can be copied from previous jobs).

You can create any helper functions in parse_dhcp_snooping_functions.py,
not just those called from parse_dhcp_snooping.py.

Check all operations:

* creating a database
* adding information about switches
* adding information based on the output of sh ip dhcp snooping binding from files
* selection of information from the database (by parameter and all information)

To make it easier to understand what the script call will look like,
here are some examples. The examples show the option when the database has active
and last_active fields, but you can also use the option without these fields.

::

    $ python parse_dhcp_snooping.py get -h
    usage: parse_dhcp_snooping.py get [-h] [--db DB_FILE]
                                      [-k {mac,ip,vlan,interface,switch}]
                                      [-v VALUE] [-a]

    optional arguments:
      -h, --help            show this help message and exit
      --db DB_FILE          database name
      -k {mac,ip,vlan,interface,switch}
                            parameter for searching records
      -v VALUE              parameter value
      -a                    show all database content


    $ python parse_dhcp_snooping.py add -h
    usage: parse_dhcp_snooping.py add [-h] [--db DB_FILE] [-s]
                                      filename [filename ...]

    positional arguments:
      filename      file(s) to add

    optional arguments:
      -h, --help    show this help message and exit
      --db DB_FILE  database name
      -s            if the flag is set, add switch data, otherwise add DHCP
                    records


    $ python parse_dhcp_snooping.py create_db
    Creating a dhcp_snooping.db database with dhcp_snooping_schema.sql schema
    Creating database...


    $ python parse_dhcp_snooping.py add sw[1-3]_dhcp_snooping.txt
    Adding information from files
    sw1_dhcp_snooping.txt, sw2_dhcp_snooping.txt, sw3_dhcp_snooping.txt

    Adding data on DHCP records to dhcp_snooping.db


    $ python parse_dhcp_snooping.py add -s switches.yml
    Adding switch data

    $ python parse_dhcp_snooping.py get
    The dhcp table has the following entries:

    Active entries:

    -----------------  ---------------  --  ----------------  ---  -  -------------------
    00:09:BB:3D:D6:58  10.1.10.2        10  FastEthernet0/1   sw1  1  2019-03-08 16:47:52
    00:04:A3:3E:5B:69  10.1.5.2          5  FastEthernet0/10  sw1  1  2019-03-08 16:47:52
    00:05:B3:7E:9B:60  10.1.5.4          5  FastEthernet0/9   sw1  1  2019-03-08 16:47:52
    00:07:BC:3F:A6:50  10.1.10.6        10  FastEthernet0/3   sw1  1  2019-03-08 16:47:52
    00:09:BC:3F:A6:50  192.168.100.100   1  FastEthernet0/7   sw1  1  2019-03-08 16:47:52
    00:A9:BB:3D:D6:58  10.1.10.20       10  FastEthernet0/7   sw2  1  2019-03-08 16:47:52
    00:B4:A3:3E:5B:69  10.1.5.20         5  FastEthernet0/5   sw2  1  2019-03-08 16:47:52
    00:C5:B3:7E:9B:60  10.1.5.40         5  FastEthernet0/9   sw2  1  2019-03-08 16:47:52
    00:A9:BC:3F:A6:50  10.1.10.60       20  FastEthernet0/2   sw2  1  2019-03-08 16:47:52
    00:E9:BC:3F:A6:50  100.1.1.6         3  FastEthernet0/20  sw3  1  2019-03-08 16:47:52
    -----------------  ---------------  --  ----------------  ---  -  -------------------


    $ python parse_dhcp_snooping.py get -k vlan -v 10
    Data from the database: dhcp_snooping.db
    Information about devices with the following parameters: vlan 10

    Active entries:

    -----------------  ----------  --  ---------------  ---  -  -------------------
    00:09:BB:3D:D6:58  10.1.10.2   10  FastEthernet0/1  sw1  1  2019-03-08 16:47:52
    00:07:BC:3F:A6:50  10.1.10.6   10  FastEthernet0/3  sw1  1  2019-03-08 16:47:52
    00:A9:BB:3D:D6:58  10.1.10.20  10  FastEthernet0/7  sw2  1  2019-03-08 16:47:52
    -----------------  ----------  --  ---------------  ---  -  -------------------


    $ python parse_dhcp_snooping.py get -k vln -v 10
    usage: parse_dhcp_snooping.py get [-h] [--db DB_FILE]
                                      [-k {mac,ip,vlan,interface,switch}]
                                      [-v VALUE] [-a]
    parse_dhcp_snooping.py get: error: argument -k: invalid choice: 'vln' (choose from 'mac', 'ip', 'vlan', 'interface', 'switch')

