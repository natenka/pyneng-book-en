.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 17.1
~~~~~~~~~~~~

Create the write_dhcp_snooping_to_csv function, which processes the output
of the show dhcp snooping binding command from different files and writes
the processed data to the csv file.

Function arguments:

* filenames - list of filenames with "show dhcp snooping binding" command output
* output - the name of the csv file into which the result will be written

The function returns None.

For example, if a list with one file sw3_dhcp_snooping.txt was passed as an argument:

::

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:E9:BC:3F:A6:50   100.1.1.6        76260       dhcp-snooping   3    FastEthernet0/20
    00:E9:22:11:A6:50   100.1.1.7        76260       dhcp-snooping   3    FastEthernet0/21
    Total number of bindings: 2

The resulting csv file should contain the following content:

::

    switch,mac,ip,vlan,interface
    sw3,00:E9:BC:3F:A6:50,100.1.1.6,3,FastEthernet0/20
    sw3,00:E9:22:11:A6:50,100.1.1.7,3,FastEthernet0/21

The first column in the csv file, the name of the switch, must be obtained from
the file name, the rest - from the contents in the files.

Check the function on the contents of the files sw1_dhcp_snooping.txt,
sw2_dhcp_snooping.txt, sw3_dhcp_snooping.txt.


Task 17.2
~~~~~~~~~~~~

In this task you need:

* take the contents of several files with the output of the sh version command
* parse command output using regular expressions and get device information
* write this information to a file in CSV format

To complete the task, you need to create two functions.

parse_sh_version function:

* expects the output of the sh version command as an argument in single string (not a filename)
* processes output using regular expressions
* returns a tuple of three elements:

  * ios - "12.4(5)T"
  * image - "flash:c2800-advipservicesk9-mz.124-5.T.bin"
  * uptime - "5 days, 3 hours, 3 minutes"

The write_inventory_to_csv function must have two parameters:

* data_filenames - expects a list of filenames as an argument with
  the output of sh version
* csv_filename - expects as an argument the name of a file (for example,
  routers_inventory.csv) to which information will be written in CSV format

write_inventory_to_csv function writes the contents to a file, in CSV format and
returns nothing.

The write_inventory_to_csv function should do the following:

* process information from each file with sh version output:
  sh_version_r1.txt, sh_version_r2.txt, sh_version_r3.txt
* using the parse_sh_version function, ios, image, uptime information
  should be obtained from each output
* from the file name you need to get the hostname
* after that all information should be written to a CSV file

The routers_inventory.csv file should have the following columns (in this order):
hostname, ios, image, uptime

The code below has created a list of files using the glob module.
You can uncomment the print(sh_version_files) line to see the content of the list.

In addition, a list of headers has been created, which should be written to CSV.

.. code:: python


    import glob

    sh_version_files = glob.glob("sh_vers*")
    #print(sh_version_files)

    headers = ["hostname", "ios", "image", "uptime"]

Task 17.3
~~~~~~~~~~~~

Create a function parse_sh_cdp_neighbors that processes the output of
the show cdp neighbors command.

The function expects, as an argument, the output of the command
as a single string (not a filename).
The function should return a dictionary that describes the connections between devices.

For example, if the following output was passed as an argument:

::

    R4>show cdp neighbors

    Device ID    Local Intrfce   Holdtme     Capability       Platform    Port ID
    R5           Fa 0/1          122           R S I           2811       Fa 0/1
    R6           Fa 0/2          143           R S I           2811       Fa 0/0

The function should return a dictionary like this:

.. code:: python

    {"R4": {"Fa 0/1": {"R5": "Fa 0/1"},
            "Fa 0/2": {"R6": "Fa 0/0"}}}

Interfaces must be written with a space. That is, so Fa 0/0, and not so Fa0/0.

Check the function on the contents of the sh_cdp_n_sw1.txt file

Task 17.3a
~~~~~~~~~~~~~

Create a generate_topology_from_cdp function that processes the
show cdp neighbor command output from multiple files and writes
the resulting topology to a single dictionary.

The generate_topology_from_cdp function must be created with parameters:

* list_of_files - list of files from which to read the output of the sh cdp neighbor command
* save_to_filename is the name of the YAML file where the topology will be saved.

  * default is None. By default, the topology is not saved to a file.
  * topology is saved only if save_to_filename is file name as argument


The function should return a dictionary that describes the connections between
devices, regardless of whether the topology is saved to a file.

Dictionary example:

.. code:: python

    {"R4": {"Fa 0/1": {"R5": "Fa 0/1"},
            "Fa 0/2": {"R6": "Fa 0/0"}},
     "R5": {"Fa 0/1": {"R4": "Fa 0/1"}},
     "R6": {"Fa 0/0": {"R4": "Fa 0/2"}}}

Interfaces must be written with a space. That is, so Fa 0/0, and not so Fa0/0.

Check the work of the generate_topology_from_cdp function on the list of files:

* sh_cdp_n_sw1.txt
* sh_cdp_n_r1.txt
* sh_cdp_n_r2.txt
* sh_cdp_n_r3.txt
* sh_cdp_n_r4.txt
* sh_cdp_n_r5.txt
* sh_cdp_n_r6.txt

Check the operation of the save_to_filename parameter and write the resulting
dictionary to the topology.yaml file. You will need it in the next task.

Task 17.3b
~~~~~~~~~~~~~

Create a transform_topology function that converts the topology to a format
suitable for the draw_topology function.

The function expects a YAML filename as an argument in which the topology is stored.

The function must read data from the YAML file, transform it accordingly,
so that the function returns a dictionary of the following form:

.. code:: python

    {("R4", "Fa 0/1"): ("R5", "Fa 0/1"),
     ("R4", "Fa 0/2"): ("R6", "Fa 0/0")}


The transform_topology function should not only change the format of the topology
representation, but also remove the "duplicate" connections (they are best seen
in the diagram that the draw_topology function generates from
the draw_network_graph.py file).
"Duplicate" connections are connections of this kind:

.. code:: python

    ("R1", "Eth0/0"): ("SW1", "Eth0/1")
    ("SW1", "Eth0/1"): ("R1", "Eth0/0")

Due to the fact that the same link is described twice, there will be extra
connections on the diagram. The task is to leave only one of these links
in the final dictionary, does not matter which one.

Check the operation of the function on the topology.yaml file (must be created
in task 17.3a). Based on the resulting dictionary, you need to generate a topology
image using the draw_topology function.
Do not copy draw_topology function code from draw_network_graph.py file.

The result should look the same as the diagram in the task_17_3b_topology.svg file:

* Interfaces must be written with a space Fa 0/0
* The arrangement of devices on the diagram may be different
* Connections must match the diagram
* There should be no "duplicate" links on the diagram

.. figure:: https://raw.githubusercontent.com/natenka/pyneng-examples-exercises/master/exercises/17_serialization/task_17_3b_topology.png


.. note::

    To complete this task, graphviz must be installed:
    apt-get install graphviz

    And a python module to work with graphviz:
    pip install graphviz

Task 17.4
~~~~~~~~~~~~

Create function write_last_log_to_csv.

Function arguments:

* source_log - the name of the csv file from which the data is read (mail_log.csv)
* output - the name of the csv file into which the result will be written

The function returns None.

The write_last_log_to_csv function processes the csv file mail_log.csv.
The mail_log.csv file contains the logs of the username change.
User cannot change email, only username.

The write_last_log_to_csv function should select from the mail_log.csv file
only the most recent entries for each user and write them to another csv file.
In the output file, the first line should be the column headers as in the source_log file.

For some users, there is only one record, and then it is necessary to write
to the final file only her. For some users, there are multiple entries with
different names. For example, a user with email c3po@gmail.com changed his
username several times:

::

    C=3PO,c3po@gmail.com,16/12/2019 17:10
    C3PO,c3po@gmail.com,16/12/2019 17:15
    C-3PO,c3po@gmail.com,16/12/2019 17:24

Of these three records, only one should be written to the final file - the most recent:

::

    C-3PO,c3po@gmail.com,16/12/2019 17:24

It is convenient to use datetime objects from the datetime module
for comparing dates. To make it easier to work with dates,
the convert_str_to_datetime function has been created - it converts a date
string in the format 11/10/2019 14:05 into a datetime object.
The resulting datetime objects can be compared with each other.
The second function, convert_datetime_to_str, does the opposite — it turns
a datetime object into a string.

It is not necessary to use the functions convert_str_to_datetime and convert_datetime_to_str


.. code:: python

    import datetime


    def convert_str_to_datetime(datetime_str):
        """
        Converts a date string formatted as 11/10/2019 14:05 to a datetime object.
        """
        return datetime.datetime.strptime(datetime_str, "%d/%m/%Y %H:%M")


    def convert_datetime_to_str(datetime_obj):
        """
        Converts a datetime object to a date string in the format 11/10/2019 14:05.
        """
        return datetime.datetime.strftime(datetime_obj, "%d/%m/%Y %H:%M")
