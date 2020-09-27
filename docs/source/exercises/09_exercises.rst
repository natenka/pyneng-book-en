.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 9.1
~~~~~~~~~~~

Create a function that generates configuration for access ports.

Function expects such arguments:

1. dictionary with interface-VLAN mapping:

.. code:: python

    {"FastEthernet0/12": 10,
     "FastEthernet0/14": 11,
     "FastEthernet0/16": 17}

2. access port configuration template in the form of a list of commands (access_mode_template list)

Function should return a list of all ports in access mode with configuration based on template access_mode_template. There should be no line feed at the end of lines in the list.

In this task, the blank for function is already done and only body of function need to be written.


Example of resulting list (line feed after each item is made for ease of reading):

.. code:: python

    [
    "interface FastEthernet0/12",
    "switchport mode access",
    "switchport access vlan 10",
    "switchport nonegotiate",
    "spanning-tree portfast",
    "spanning-tree bpduguard enable",
    "interface FastEthernet0/17",
    "switchport mode access",
    "switchport access vlan 150",
    "switchport nonegotiate",
    "spanning-tree portfast",
    "spanning-tree bpduguard enable",
    ...]

Check function with access_config dictionary.

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    access_mode_template = [
        "switchport mode access", "switchport access vlan",
        "switchport nonegotiate", "spanning-tree portfast",
        "spanning-tree bpduguard enable"
    ]

    access_config = {
        "FastEthernet0/12": 10,
        "FastEthernet0/14": 11,
        "FastEthernet0/16": 17
    }


    def generate_access_config(intf_vlan_mapping, access_template):
        """
        intf_vlan_mapping - dictionary with interface-VLAN mapping:
            {"FastEthernet0/12": 10,
             "FastEthernet0/14": 11,
             "FastEthernet0/16": 17}
        access_template - list of commands for port in access mode

        Return list of all ports in access mode with configuration based on temlate 
        """


Task 9.1a
~~~~~~~~~~~~

Make a copy of generate_access_config() function from task 9.1.

Complete script: enter an additional parameter that controls whether port-security will be configured:

* parameter name "psecurity"
* default is None
* to configure port-security, list of *port-security* commands should be passed as a value (in port_security_template list)

Function should return a list of all ports in access mode with configuration based on access_mode_template template and template port_security_template template if it has been passed. There should be no line feed at the end of lines in the list.


Check function with access_config dictionary, with and without port-security configuration generation.

Example of a function call:

::

    print(generate_access_config(access_config, access_mode_template))
    print(generate_access_config(access_config, access_mode_template, port_security_template))


Restriction: All tasks must be performed using only covered topics.

.. code:: python

    access_mode_template = [
        "switchport mode access", "switchport access vlan",
        "switchport nonegotiate", "spanning-tree portfast",
        "spanning-tree bpduguard enable"
    ]

    port_security_template = [
        "switchport port-security maximum 2",
        "switchport port-security violation restrict",
        "switchport port-security"
    ]

    access_config = {"FastEthernet0/12": 10, "FastEthernet0/14": 11, "FastEthernet0/16": 17}


Task 9.2
~~~~~~~~~~~

Create a generate_trunk_config() function that generates configuration for trunk ports.

Function should have such parameters:

1. intf_vlan_mapping: expects a dictionary with interface-VLAN mapping:

.. code:: python

    {"FastEthernet0/1": [10, 20],
     "FastEthernet0/2": [11, 30],
     "FastEthernet0/4": [17]}

2. trunk_template: expects trunk port configuration template as command list (trunk_mode_template list)

Function should return a list of commands with configuration based on specified ports and trunk_mode_template template. There should be no line feed at the end of lines in the list.

Check function with trunk_config dictionary and list of commands trunk_mode_template. If this check is successful, check function again with trunk_config_2 dictionary and make sure that the resulting list contains correct interface and vlan numbers.


Example of resulting list (line feed after each item is made for ease of reading):

.. code:: python

    [
    "interface FastEthernet0/1",
    "switchport mode trunk",
    "switchport trunk native vlan 999",
    "switchport trunk allowed vlan 10,20,30",
    "interface FastEthernet0/2",
    "switchport mode trunk",
    "switchport trunk native vlan 999",
    "switchport trunk allowed vlan 11,30",
    ...]


Restriction: All tasks must be performed using only covered topics.

.. code:: python

    trunk_mode_template = [
        "switchport mode trunk", "switchport trunk native vlan 999",
        "switchport trunk allowed vlan"
    ]

    trunk_config = {
        "FastEthernet0/1": [10, 20, 30],
        "FastEthernet0/2": [11, 30],
        "FastEthernet0/4": [17]
    }


Task 9.2a
~~~~~~~~~~~~

Make a copy of generate_trunk_config() function from task 9.2.

Change function to return a dictionary rather than a list of commands:

* keys: interface names like "FastEthernet0/1"
* values: list of commands to execute on this interface

Check function with trunk_config dictionary and trunk_mode_template template.

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    trunk_mode_template = [
        "switchport mode trunk", "switchport trunk native vlan 999",
        "switchport trunk allowed vlan"
    ]

    trunk_config = {
        "FastEthernet0/1": [10, 20, 30],
        "FastEthernet0/2": [11, 30],
        "FastEthernet0/4": [17]
    }


Task 9.3
~~~~~~~~~~~

Create get_int_vlan_map() function that processes switch configuration file and returns a tuple with two dictionaries:

1. dictionary of ports in access mode, where keys are port numbers and values is access VLAN (numbers):

.. code:: python

    {"FastEthernet0/12": 10,
     "FastEthernet0/14": 11,
     "FastEthernet0/16": 17}

2. dictionary of ports in trunk mode, where keys are port number and values are list of allowed VLANs (list of numbers):

.. code:: python
    {"FastEthernet0/1": [10, 20],
     "FastEthernet0/2": [11, 30],
     "FastEthernet0/4": [17]}

Function should have one parameter - config_filename, that expects as an argument the name of configuration file.

Check function with config_sw1.txt file

Restriction: All tasks must be performed using only covered topics.


Task 9.3a
~~~~~~~~~~~~

Copy get_int_vlan_map() function from task 9.3.

Complete function: add configuration support when access port configuration is like:

::

    interface FastEthernet0/20
     switchport mode access
     duplex auto

That is, port is in VLAN 1

In this case, port dictionary should add information that port in VLAN 1

.. code:: python
      {"FastEthernet0/12": 10,
       "FastEthernet0/14": 11,
       "FastEthernet0/20": 1}

Function should have one parameter - config_filename, that expects as an argument the name of configuration file.

Check function with config_sw2.txt

Restriction: All tasks must be performed using only covered topics.


Task 9.4
~~~~~~~~~~~

Create convert_config_to_dict() function that processes switch configuration file and returns dictionary:

* All top-level commands (global configuration mode) will be the keys.
* If top-level command has a sub-command, it must be in value of corresponding key as a list (spaces at the beginning of line should be removed).
* If top level command does not have a sub-command, the value is an empty list

Function should have one parameter - config_filename, that expects as an argument the name of configuration file.

When processing a configuration file, you should ignore lines that start with "!" as well as the lines that contain words from *ignore* list. To check whether to ignore a line, use ignore_command() function.

Check function with config_sw1.txt file

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]


    def ignore_command(command, ignore):
        """
        Function checks whether command words from *ignore* list.

        command - string. Command that should be checked.
        ignore - list. List of words.

        Returns
        * True, if command contains a word from *ignore* list.
        * False - if not
        """
        return any(word in command for word in ignore)

