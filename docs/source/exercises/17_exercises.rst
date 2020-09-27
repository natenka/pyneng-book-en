.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 17.1
~~~~~~~~~~~~


Create write_dhcp_snooping_to_csv() function that processes the output of show dhcp snooping binding command from different files and writes processed data to csv file.

Function arguments:

* filenames - list with file names with show dhcp snooping binding outputs
* output - file name in csv format to which the result will be written

Function does not return anything.

For example, if a list with one sw3_dhcp_snooping.txt file was passed as an argument:

::

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:E9:BC:3F:A6:50   100.1.1.6        76260       dhcp-snooping   3    FastEthernet0/20
    00:E9:22:11:A6:50   100.1.1.7        76260       dhcp-snooping   3    FastEthernet0/21
    Total number of bindings: 2

The resulting csv file should have such content:

::

    switch,mac,ip,vlan,interface
    sw3,00:E9:BC:3F:A6:50,100.1.1.6,3,FastEthernet0/20
    sw3,00:E9:22:11:A6:50,100.1.1.7,3,FastEthernet0/21


Check function with content of sw1_dhcp_snooping.txt, sw2_dhcp_snooping.txt, sw3_dhcp_snooping.txt files. The first column in a csv file should be retrieved from filename, the rest from contents of files.


Task 17.2
~~~~~~~~~~~~

This task requires:

* take contents of several files with sh version command output
* parse command output with regular expressions and get device information
* write the received information to a CSV file

To complete task you need to create two functions.

Function parse_sh_version():

* expects as an argument the output of sh version command as one string (not file name)
* handles output using regular expressions
* returns a tuple of three elements:

  * ios - in format "12.4(5)T"
  * image - in format "flash:c2800-advipservicesk9-mz.124-5.T.bin"
  * uptime - in format "5 days, 3 hours, 3 minutes"

Function write_inventory_to_csv() should have two parameters:

* data_filenames - expects as argument list of file names with sh version command output
* csv_filename - expects as argument the name of file (for example, routers_inventory.csv) into which information will be written in CSV format

Function write_inventory_to_csv() writes content to a CSV file and returns nothing


Function write_inventory_to_csv() should do the following:

* Process information from each file with sh version output:

  * sh_version_r1.txt, sh_version_r2.txt, sh_version_r3.txt

* with parse_sh_version() function, information like ios, image, uptime should be obtained from each output
* hostname should be received from file name
* then all information should be written in CSV file

File routers_inventory.csv should have such columns: ``hostname, ios, image, uptime``

In script, using glob module, a list of files whose name begins on *sh_vers* is created. You can uncomment string *print(sh_version_files)* to see the contents of list.

In addition, a list of headers was created that should be written in CSV.

.. code:: python


    import glob

    sh_version_files = glob.glob("sh_vers*")
    #print(sh_version_files)

    headers = ["hostname", "ios", "image", "uptime"]

Task 17.3
~~~~~~~~~~~~

Create parse_sh_cdp_neighbors() function that handles the output of show cdp neighbors command.

Function expects as an argument the output of command as a string (not file name). Function should return a dictionary that describes connections between devices.

For example, if such output is given as an argument:

::

    R4>show cdp neighbors

    Device ID    Local Intrfce   Holdtme     Capability       Platform    Port ID
    R5           Fa 0/1          122           R S I           2811       Fa 0/1
    R6           Fa 0/2          143           R S I           2811       Fa 0/0

Function should return such dictionary:

.. code:: python

    {"R4": {"Fa 0/1": {"R5": "Fa 0/1"},
            "Fa 0/2": {"R6": "Fa 0/0"}}}

Interfaces should be written with space. That is Fa 0/0, not Fa0/0.

Check function with contents of sh_cdp_n_sw1.txt file.

Task 17.3a
~~~~~~~~~~~~~

Create generate_topology_from_cdp() function that handles the output of show cdp neighbor command from multiple files and writes the resulting topology in one dictionary.

Function generate_topology_from_cdp() should be created with parameters:

* list_of_files - list of files from which to read the output of sh cdp neighbor command
* save_to_filename - name of file in YAML format that stores topology.

  * default value - None. By default, topology is not saved to file
  * topology is saved only if file name is specified for save_to_filename argument

Function should return a dictionary that describes connections between devices, regardless of whether topology is saved to file.

The structure of dictionary should be:

.. code:: python

    {"R4": {"Fa 0/1": {"R5": "Fa 0/1"},
            "Fa 0/2": {"R6": "Fa 0/0"}},
     "R5": {"Fa 0/1": {"R4": "Fa 0/1"}},
     "R6": {"Fa 0/0": {"R4": "Fa 0/2"}}}

Interfaces should be written with space. That is Fa 0/0, not Fa0/0.

Check generate_topology_from_cdp() function with list of files:

* sh_cdp_n_sw1.txt
* sh_cdp_n_r1.txt
* sh_cdp_n_r2.txt
* sh_cdp_n_r3.txt
* sh_cdp_n_r4.txt
* sh_cdp_n_r5.txt
* sh_cdp_n_r6.txt

Check save_to_filename option and write the resulting dictionary to topology.yaml file.

Task 17.3b
~~~~~~~~~~~~~

Create transform_topology() function that converts topology into a format suitable for draw_topology() function.

Function expects as argument the name of file in YAML format in which topology is stored.

Function should read data from YAML file, convert it accordingly, so that the function returns a dictionary of this type:

.. code:: python

    {("R4", "Fa 0/1"): ("R5", "Fa 0/1"),
     ("R4", "Fa 0/2"): ("R6", "Fa 0/0")}

Function transform_topology() should not only change the format of topology representation but also remove duplicate connections (these are best seen in diagram generated by draw_topology).

Check function with topology.yaml file (should be created in previous task 17.2a). Based on resulting dictionary, you should generate a topology image using draw_topology() function. Do not copy draw_topology() function code.

Result should look the same as scheme in task_17_3b_topology.svg file

At the same time:

* Interfaces should be written with space Fa 0/0
* The arrangement of devices on diagram may be different
* Connections should follow the diagram
* There should be no duplicate links on diagram

.. note::

    To complete this task, graphviz should be installed:
    apt-get install graphviz

    And python module for working with graphviz:
    pip install graphviz


.. figure:: https://raw.githubusercontent.com/natenka/pyneng-examples-exercises/master/exercises/17_serialization/task_17_3b_topology.png

Task 17.4
~~~~~~~~~~~~

Create write_last_log_to_csv() function.

Function arguments:

* source_log - name of csv file from which data is read (example mail_log.csv)
* output - name of csv file to which the result will be written

Function does not return anything.

Function write_last_log_to_csv() handles mail_log.csv file. File mail_log.csv contains logs of user name changes. At the same time, user cannot change email, only name.

Function write_last_log_to_csv() should select only the most recent entries for each user from mail_log.csv file and write them to another csv file. The *output* file should have column headers as a first line, similar to source_log file.

Some users have only one record and then only it should be written to final file. Some users have multiple entries with different names. For example, user with c3po@gmail.com has changed his name several times:

::

    C=3PO,c3po@gmail.com,16/12/2019 17:10
    C3PO,c3po@gmail.com,16/12/2019 17:15
    C-3PO,c3po@gmail.com,16/12/2019 17:24

Of these three entries, only one should be written to final file - the latest:

::

    C-3PO,c3po@gmail.com,16/12/2019 17:24

It is convenient to use datetime objects from *datetime* module to compare dates. To simplify work with dates, convert_str_to_datetime() function was created - it converts a string with a date in format 11/10/2019 14:05 into an datetime object. The resulting datetime objects can be compared. The second convert_datetime_to_str() function does the reverse operation - converts datetime object into a string.

Functions convert_str_to_datetime() and convert_datetime_to_str() are not necessary to use.

.. code:: python

    import datetime


    def convert_str_to_datetime(datetime_str):
        """
        Converts a string with a date in format 11/10/2019 14:05 into an datetime object.
        """
        return datetime.datetime.strptime(datetime_str, "%d/%m/%Y %H:%M")


    def convert_datetime_to_str(datetime_obj):
        """
        Converts datetime object into a string with a date in format 11/10/2019 14:05.
        """
        return datetime.datetime.strftime(datetime_obj, "%d/%m/%Y %H:%M")

