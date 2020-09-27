.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 25.1
~~~~~~~~~~~~

There are no tests for tasks of section 25!

Two scripts need to be created:

1. create_db.py
2. add_data.py

Code in scripts should be split into functions. You should decide what functions to create and how to divide code. Part of code could be global.


1. create_db.py - this script should have functionality to create database:

  * DB file existence verification should be performed
  * if there is no file, according to description of database scheme in the dhcp_snooping_schema.sql file, database should be created
  * database file name - dhcp_snooping.db

Database should have two tables (scheme is described in dhcp_snooping_schema.sql):

 * switches - it contains switches data
 * dhcp - it contains information derived from the output of *sh ip dhcp snooping binding* command

Example of script execution when there is no dhcp_snooping.db file:

:: 

    $ python create_db.py
    Building a database ...

After file creation:

:: 

    $ python create_db.py
    Database exists


2. add_data.py - this script adds data to database. Script should add data from output of *sh ip dhcp snooping binding* and information about switches

Accordingly, add_data.py file should have two parts:

* switch information is added to *switches* table

  * switches data, located in switches.yml file

* information based on output of *sh ip dhcp snooping binding* is added to *dhcp* table 

  * output from three switches: files sw1_dhcp_snooping.txt, sw2_dhcp_snooping.txt, sw3_dhcp_snooping.txt
  * since *dhcp* table has changed and now has a *switch* field, it also needs to be filled in. Switch name is defined by the name of data file

Example of script run when a database has not yet been created:

::

    $ python add_data.py
    Database does not exist. Before adding data, you should create it

Example of first time script run after creating a database:

::

    $ python add_data.py
    Add data to switches table...
    Adding data to dhcp table...

Example of script execution after data has been added to table (order in which data is added may be arbitrary, but messages should be displayed in the same way as output below):

::

    $ python add_data.py
    Add data to switches table...
    When adding data: ('sw1', 'London, 21 New Globe Walk') Error occurred: UNIQUE constraint failed: switches.hostname
    When adding data: ('sw2', 'London, 21 New Globe Walk') Error occurred: UNIQUE constraint failed: switches.hostname
    When adding data: ('sw3', 'London, 21 New Globe Walk') Error occurred: UNIQUE constraint failed: switches.hostname
    Adding data to dhcp table..
    When adding data: ('00:09:BB:3D:D6:58', '10.1.10.2', '10', 'FastEthernet0/1', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:04:A3:3E:5B:69', '10.1.5.2', '5', 'FastEthernet0/10', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:05:B3:7E:9B:60', '10.1.5.4', '5', 'FastEthernet0/9', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:07:BC:3F:A6:50', '10.1.10.6', '10', 'FastEthernet0/3', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:09:BC:3F:A6:50', '192.168.100.100', '1', 'FastEthernet0/7', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:E9:BC:3F:A6:50', '100.1.1.6', '3', 'FastEthernet0/20', 'sw3') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:E9:22:11:A6:50', '100.1.1.7', '3', 'FastEthernet0/21', 'sw3') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:A9:BB:3D:D6:58', '10.1.10.20', '10', 'FastEthernet0/7', 'sw2') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:B4:A3:3E:5B:69', '10.1.5.20', '5', 'FastEthernet0/5', 'sw2') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:C5:B3:7E:9B:60', '10.1.5.40', '5', 'FastEthernet0/9', 'sw2') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:A9:BC:3F:A6:50', '10.1.10.60', '20', 'FastEthernet0/2', 'sw2') Error occurred: UNIQUE constraint failed: dhcp.mac


Both scripts are called without argument.

Task 25.2
~~~~~~~~~~~~

There are no tests for tasks of section 25!

In this task you need to create get_data.py script.

Code in script should be split into functions. You should decide what functions to create and how to divide code. Part of code could be global.

Script can be passed arguments and depending on arguments, different information should be displayed. If script is called:

* without arguments, output the entire contents of *dhcp* table
* with two arguments, output information from *dhcp* table which corresponds to field and value
* with any other number of arguments, output message that script supports only two or zero arguments

Database file can be copied from task 25.1.

Examples of output for different amount and value of arguments:

::

    $ python get_data.py
    dhcp table has such entries:
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

    Information on devices with such parameters: vlan 10
    -----------------  ----------  --  ---------------  ---
    00:09:BB:3D:D6:58  10.1.10.2   10  FastEthernet0/1  sw1
    00:07:BC:3F:A6:50  10.1.10.6   10  FastEthernet0/3  sw1
    00:A9:BB:3D:D6:58  10.1.10.20  10  FastEthernet0/7  sw2
    -----------------  ----------  --  ---------------  ---

    $ python get_data.py ip 10.1.10.2

    Information on devices with such parameters: ip 10.1.10.2
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

There are no tests for tasks of section 25!

In past tasks information was added to empty database. In this task, the situation when database already has information is considered.

Copy add_data.py script from task 25.1 and try to run it again on existing database.
The result should be:

::

    $ python add_data.py
    Add data to switches table...
    When adding data: ('sw1', 'London, 21 New Globe Walk') Error occurred: UNIQUE constraint failed: switches.hostname
    When adding data: ('sw2', 'London, 21 New Globe Walk') Error occurred: UNIQUE constraint failed: switches.hostname
    When adding data: ('sw3', 'London, 21 New Globe Walk') Error occurred: UNIQUE constraint failed: switches.hostname
    Adding data to dhcp table..
    When adding data: ('00:09:BB:3D:D6:58', '10.1.10.2', '10', 'FastEthernet0/1', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:04:A3:3E:5B:69', '10.1.5.2', '5', 'FastEthernet0/10', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:05:B3:7E:9B:60', '10.1.5.4', '5', 'FastEthernet0/9', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:07:BC:3F:A6:50', '10.1.10.6', '10', 'FastEthernet0/3', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    When adding data: ('00:09:BC:3F:A6:50', '192.168.100.100', '1', 'FastEthernet0/7', 'sw1') Error occurred: UNIQUE constraint failed: dhcp.mac
    ... (output ommited)

When creating a database schema, it was explicitly stated that MAC address field should be unique. Therefore, when an entry with the same MAC address is added an exception (error) occurs. In task 25.1, exception is processed and message is displayed on standard output stream.

In this task it is considered that information is periodically read from switches and written into files. After that, information from files should be transmitted to database. However, there may be changes in new data: MAC is missing, MAC has moved to another port/vlan, new MAC has appeared, etc.

In this task in *dhcp* table it is necessary to create a new *active* field that will indicate whether the entry is relevant. New database schema is in dhcp_snooping_schema.sql file.

Field *active* should take such values:

* 0 - means False. Used to mark the entry as inactive
* 1 - True. Used to indicate that the entry is active

Each time the information from DHCP snooping output files is re-added, all existing entries (for this switch) should be marked as inactive (active = 0). You can then update information and mark new entries as active (active = 1).

Thus, old entries will remain in database for MAC addresses that are not currently active and updated information for active addresses will appear.

For example, there are such entries in *dhcp* table:

::

    mac                ip          vlan        interface         switch      active
    -----------------  ----------  ----------  ----------------  ----------  ----------
    00:09:BB:3D:D6:58  10.1.10.2   10          FastEthernet0/1   sw1         1
    00:04:A3:3E:5B:69  10.1.5.2    5           FastEthernet0/10  sw1         1
    00:05:B3:7E:9B:60  10.1.5.4    5           FastEthernet0/9   sw1         1
    00:07:BC:3F:A6:50  10.1.10.6   10          FastEthernet0/3   sw1         1
    00:09:BC:3F:A6:50  192.168.10  1           FastEthernet0/7   sw1         1


And you have to add this information from file:

::

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:09:BB:3D:D6:58   10.1.10.2        86250       dhcp-snooping   10    FastEthernet0/1
    00:04:A3:3E:5B:69   10.1.15.2        63951       dhcp-snooping   15    FastEthernet0/15
    00:05:B3:7E:9B:60   10.1.5.4         63253       dhcp-snooping   5     FastEthernet0/9
    00:07:BC:3F:A6:50   10.1.10.6        76260       dhcp-snooping   10    FastEthernet0/5


After adding data, table should look like:

::

    mac                ip               vlan        interface         switch      active
    -----------------  ---------------  ----------  ---------------   ----------  ----------
    00:09:BC:3F:A6:50  192.168.100.100  1           FastEthernet0/7   sw1         0
    00:09:BB:3D:D6:58  10.1.10.2        10          FastEthernet0/1   sw1         1
    00:04:A3:3E:5B:69  10.1.15.2        15          FastEthernet0/15  sw1         1
    00:05:B3:7E:9B:60  10.1.5.4         5           FastEthernet0/9   sw1         1
    00:07:BC:3F:A6:50  10.1.10.6        10          FastEthernet0/5   sw1         1

New information should overwrite previous information:

* MAC 00:04:A3:3E:5B:69 moved to another port and got another interface and got a different address
* MAC 00:07:BC:3F:A6:50 moved to another port

If there is no MAC address in new file, it should be left in database with active = 0: MAC-адреса 00:09:BC:3F:A6:50 50 is not in new information (computer is turned off).


Change add_data.py script to meet new conditions and fill in *active* field.

Code in script should be split into functions. You should decide what functions to create and how to divide code. Part of code could be global.

> To check the correctness of SQL query you can execute it in command line using sqlite3 utility.

To check task and work of new field, first add information from sw*_dhcp_snooping.txt files to database and then add information from new_data/sw*_dhcp_snooping.txt files

Data should look like this (the order of lines can be any)

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

There are no tests for tasks of section 25!

Copy get_data file from 25.2 task. Add *active* column to script that we added to 25.3 task.

Now, when information is requested, active entries should be displayed first and then inactive entries. If there are no inactive entries, do not display title "Inactive entries".

Examples of resulting script execution:

::

    $ python get_data.py
    dhcp table has such entries:

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

    Information on devices with such parameters:  vlan 5

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

    Information on devices with such parameters: vlan 10

    Active entries:

    -----------------  ----------  --  ---------------  ---  -
    00:09:BB:3D:D6:58  10.1.10.2   10  FastEthernet0/1  sw1  1
    00:07:BC:3F:A6:50  10.1.10.6   10  FastEthernet0/5  sw1  1
    00:A9:BB:3D:D6:58  10.1.10.20  10  FastEthernet0/7  sw2  1
    00:A9:33:44:A6:50  10.1.10.77  10  FastEthernet0/4  sw2  1
    -----------------  ----------  --  ---------------  ---  -

Task 25.5
~~~~~~~~~~~~

There are no tests for tasks of section 25!

After completing tasks 25.1 - 25.5 information about inactive entries is left in database. And if some MAC address has not appeared in new entries, the entry with it may remain in database forever.

Although it may be useful to see where MAC address was last, it is not very useful to keep this information permanently.

For example, if an entry in database is more than a month old, it can be deleted.

In order to make such a condition you need to enter a new field in which to write the most recent time of adding an entry.

New field is called *last_active* and should contain a string in format: YYYY-MM-DD HH:MM:SS.

This task requires:

* Amend *dhcp* table accordingly and add a new field.
  
  * table can be changed from cli sqlite but dhcp_snooping_schema.sql file also needs to be changed

* change add_data.py script to add time to each entry

You can get string with time and date in specified format using the datetime() function in SQL query. Syntax of the use:

.. code:: sql

    sqlite> insert into dhcp (mac, ip, vlan, interface, switch, active, last_active)
       ...> values ('00:09:BC:3F:A6:50', '192.168.100.100', '1', 'FastEthernet0/7', 'sw1', '0', datetime('now'));

That is, instead of value that is written to database you should specify datetime('now').

After this command such entry will appear in database:

::

    mac                ip               vlan   interface        switch   active   last_active
    -----------------  ---------------  -----  ---------------  -------  -------  -------------------
    00:09:BC:3F:A6:50  192.168.100.100  1      FastEthernet0/7  sw1      0        2019-03-08 11:26:56


Task 25.5a
~~~~~~~~~~~~~

There are no tests for tasks of section 25!

After completing task 25.5, *dhcp* table has a new field *last_active*.

Update add_data.py script to remove all entries that were active more than 7 days ago.

To get such entries you can simply manually update *last_active* field in some entries and set time 7 or more days.

Task file describes an example of working with datetime module objects. Shows how to get a date 7 days ago. With this date you will have to compare *last_active* time.

Note that you can compare lines with date that are written in database.

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

There are no tests for tasks of section 25!

There is a parse_dhcp_snooping.py file in this task. File parse_dhcp_snooping.py should not be changed.

File creates several functions and describes command line arguments that file accepts.

There is support for all actions that in previous tasks were executed in create_db.py, add_data.py and get_data.py.

File parse_dhcp_snooping.py has a line: 
import parse_dhcp_snooping_functions as pds

And the goal of this task is to create all necessary functions in parse_dhcp_snooping_functions.py file based on information in parse_dhcp_snooping.py.

From parse_dhcp_snooping.py file it is necessary to define:

* which functions should be in parse_dhcp_snooping_functions file.
* which parameters to create in these functions

It is necessary to create appropriate functions and transfer to them functionality described in previous tasks.

All necessary information is present in functions create(), add(), get() in parse_dhcp_snooping.py file.


To make it easier to start, try to create necessary functions in parse_dhcp_snooping_functions.py and simply display function arguments using print().

Then you can create functions that request information from database (database can be copied from previous tasks).

You can create any auxiliary functions in parse_dhcp_snooping_functions.py file, not only those that are called from parse_dhcp_snooping.py.


Check all operations:

* creation of the DB
* addition of information on switches
* add information based on the output of *sh ip dhcp snooping binding* from files
* fetching information from DB (by parameter and all information)

To make it easier to understand what a script call will look like, the following are a few examples. Examples show a variant where database has *active* and *last_active* fields, but you can also use variant without these fields.

::

    $ python parse_dhcp_snooping.py get -h
    usage: parse_dhcp_snooping.py get [-h] [--db DB_FILE]
                                      [-k {mac,ip,vlan,interface,switch}]
                                      [-v VALUE] [-a]

    optional arguments:
      -h, --help            show this help message and exit
      --db DB_FILE          database name
      -k {mac,ip,vlan,interface,switch}
                            parameter for enties search
      -v VALUE              parameter value
      -a                    show all DB content


    $ python parse_dhcp_snooping.py add -h
    usage: parse_dhcp_snooping.py add [-h] [--db DB_FILE] [-s]
                                      filename [filename ...]

    positional arguments:
      filename      file(s) to be added

    optional arguments:
      -h, --help    show this help message and exit
      --db DB_FILE  database name
      -s            if flag set, add switches data. Otherwise add DHCP records


    $ python parse_dhcp_snooping.py add -h
    usage: parse_dhcp_snooping.py add [-h] [--db DB_FILE] [-s]
                                      filename [filename ...]

    positional arguments:
      filename      file(s) to be added

    optional arguments:
      -h, --help    show this help message and exit
      --db DB_FILE  database name
      -s            if flag set, add switches data. Otherwise add DHCP records


    $ python parse_dhcp_snooping.py get -h
    usage: parse_dhcp_snooping.py get [-h] [--db DB_FILE]
                                      [-k {mac,ip,vlan,interface,switch}]
                                      [-v VALUE] [-a]

    optional arguments:
      -h, --help            show this help message and exit
      --db DB_FILE          database name
      -k {mac,ip,vlan,interface,switch}
                            parameter for enties search
      -v VALUE              parameter value
      -a                    show all DB content


    $ python parse_dhcp_snooping.py create_db
    Buidlding databse dhcp_snooping.db with schema dhcp_snooping_schema.sql
    Buidlding databse...


    $ python parse_dhcp_snooping.py add sw[1-3]_dhcp_snooping.txt
    Reading inforamtion from files
    sw1_dhcp_snooping.txt, sw2_dhcp_snooping.txt, sw3_dhcp_snooping.txt

    Adding DHCP records data to dhcp_snooping.db


    $ python parse_dhcp_snooping.py add -s switches.yml
    Adding switches data 

    $ python parse_dhcp_snooping.py get
    dhcp table has such entries:

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
    Data from DB: dhcp_snooping.db
    Information on devices with such parameters: vlan 10

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

