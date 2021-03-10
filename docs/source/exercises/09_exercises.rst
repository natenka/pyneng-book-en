.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 9.1
~~~~~~~~~~~

Create generate_access_config function that generates configuration
for access ports.

The function expects arguments:

* a dictionary with interface as a key and VLAN as a value (access_config or
  access_config_2 dict)
* access ports configuration template as a list of commands (access_mode_template list)

The function should return a list of all ports in access mode with configuration
based on the access_mode_template template.

In this task, the beginning of the function is written and you just need to
continue writing the function body itself.


An example of a final list (each string is written on a new line for readability):

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

Check the operation of the function using the access_config dictionary
and the list of commands access_mode_template.
If the previous check was successful, check the function again using the dictionary
access_config_2 and make sure that the final list contains the correct interface
numbers and vlans.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    access_mode_template = [
        "switchport mode access",
        "switchport access vlan",
        "switchport nonegotiate",
        "spanning-tree portfast",
        "spanning-tree bpduguard enable",
    ]

    access_config = {"FastEthernet0/12": 10, "FastEthernet0/14": 11, "FastEthernet0/16": 17}

    access_config_2 = {
        "FastEthernet0/3": 100,
        "FastEthernet0/7": 101,
        "FastEthernet0/9": 107,
    }


    def generate_access_config(intf_vlan_mapping, access_template):
        """
        intf_vlan_mapping is a dictionary with interface-VLAN mapping:
             {'FastEthernet0/12': 10,
              'FastEthernet0/14': 11,
              'FastEthernet0/16': 17}
        access_template - list of commands for the port in access mode

        Returns a list of commands.
        """

Task 9.1a
~~~~~~~~~~~~

Make a copy of the code from the task 9.1.

Add this functionality: add an additional parameter that controls whether
port-security configured

  * parameter name 'psecurity'
  * default is None
  * to configure port-security, a list of commands must be passed as a value
    port-security (port_security_template list)

The function should return a list of all ports in access mode with configuration
based on the access_mode_template template and the port_security_template template,
if passed. There should not be a new line character at the end of lines in the list.

Check the operation of the function using the example of the access_config
dictionary, with the generation of the configuration port-security and without.

An example of a function call:

::

    print(generate_access_config(access_config, access_mode_template))
    print(generate_access_config(access_config, access_mode_template, port_security_template))


Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    access_mode_template = [
        "switchport mode access",
        "switchport access vlan",
        "switchport nonegotiate",
        "spanning-tree portfast",
        "spanning-tree bpduguard enable",
    ]

    port_security_template = [
        "switchport port-security maximum 2",
        "switchport port-security violation restrict",
        "switchport port-security"
    ]

    access_config = {"FastEthernet0/12": 10, "FastEthernet0/14": 11, "FastEthernet0/16": 17}


Task 9.2
~~~~~~~~~~~

Create generate_trunk_config function that generates configuration
for access ports.

The function expects arguments:

* intf_vlan_mapping: expects a dictionary with interface-VLAN mapping
  (trunk_config or trunk_config_2)
* trunk_template: expects trunk port configuration template as command list
  (trunk_mode_template list)

The function should return a list of commands with configuration based on
the specified ports and trunk_mode_template.

Check the operation of the function using the example of the trunk_config
dictionary and a list of commands trunk_mode_template.
If the previous check was successful, check the function again
on the trunk_config_2 dictionary and make sure that the final list contains
the correct numbers interfaces and vlans.


An example of a final list (each string is written on a new line for readability):

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


Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    trunk_mode_template = [
        "switchport mode trunk",
        "switchport trunk native vlan 999",
        "switchport trunk allowed vlan",
    ]

    trunk_config = {
        "FastEthernet0/1": [10, 20, 30],
        "FastEthernet0/2": [11, 30],
        "FastEthernet0/4": [17],
    }

    trunk_config_2 = {
        "FastEthernet0/11": [120, 131],
        "FastEthernet0/15": [111, 130],
        "FastEthernet0/14": [117],
    }


Task 9.2a
~~~~~~~~~~~~

Make a copy of the code from the task 9.2.

Change the function so that it returns a dictionary instead of a list of commands:
- keys: interface names, like 'FastEthernet0/1'
- values: the list of commands that you need execute on this interface

Check the operation of the function using the example of the trunk_config
dictionary and the trunk_mode_template template.

An example of a final dict (each string is written on a new line for readability):

.. code:: python

    {
        "FastEthernet0/1": [
            "switchport mode trunk",
            "switchport trunk native vlan 999",
            "switchport trunk allowed vlan 10,20,30",
        ],
        "FastEthernet0/2": [
            "switchport mode trunk",
            "switchport trunk native vlan 999",
            "switchport trunk allowed vlan 11,30",
        ],
        "FastEthernet0/4": [
            "switchport mode trunk",
            "switchport trunk native vlan 999",
            "switchport trunk allowed vlan 17",
        ],
    }


Restriction: All tasks must be done using the topics covered in this and previous chapters.

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

Create a get_int_vlan_map function that handles the switch configuration
file and returns a tuple of two dictionaries:

1. A dictionary of ports in access mode, where the keys are port numbers,
   and the access VLAN values (numbers):

.. code:: python

    {"FastEthernet0/12": 10,
     "FastEthernet0/14": 11,
     "FastEthernet0/16": 17}

2. A dictionary of ports in trunk mode, where the keys are port numbers,
   and the values are the list of allowed VLANs (list of numbers):

.. code:: python

    {"FastEthernet0/1": [10, 20],
     "FastEthernet0/2": [11, 30],
     "FastEthernet0/4": [17]}

The function must have one parameter, config_filename, which expects as an argument
the name of the configuration file.

Check the operation of the function using the config_sw1.txt file.

Restriction: All tasks must be done using the topics covered in this and previous chapters.


Task 9.3a
~~~~~~~~~~~~

Make a copy of the code from the task 9.3.

Add this functionality: add support for configuration when the port is in VLAN 1
and the access port setting looks like this:

::

    interface FastEthernet0/20
        switchport mode access
        duplex auto

In this case, information should be added to the dictionary that the port in VLAN 1
Dictionary example:

.. code:: python

    {'FastEthernet0/12': 10,
     'FastEthernet0/14': 11,
     'FastEthernet0/20': 1 }

The function must have one parameter, config_filename, which expects as an argument
the name of the configuration file.

Check the operation of the function using the config_sw2.txt file.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 9.4
~~~~~~~~~~~

Create a convert_config_to_dict function that handles the switch configuration
file and returns a dictionary:

* All top-level commands (global configuration mode) will be keys.
* If the top-level team has subcommands, they must be in the value
  from the corresponding key, in the form of a list (spaces at the beginning
  of the line must be removed).
* If the top-level command has no subcommands, then the value will be an empty list

The function must have one parameter, config_filename, which expects as an argument
the name of the configuration file.

Check the operation of the function using the config_sw1.txt file.

When processing the configuration file, you should ignore the lines that begin
with '!', as well as lines containing words from the ignore list.

To check if a line should be ignored, use the ignore_command function.

The part of the dictionary that the function should return (the full output can be seen
in test_task_9_4.py test):

.. code:: python

    {
        "version 15.0": [],
        "service timestamps debug datetime msec": [],
        "service timestamps log datetime msec": [],
        "no service password-encryption": [],
        "hostname sw1": [],
        "interface FastEthernet0/0": [
            "switchport mode access",
            "switchport access vlan 10",
        ],
        "interface FastEthernet0/1": [
            "switchport trunk encapsulation dot1q",
            "switchport trunk allowed vlan 100,200",
            "switchport mode trunk",
        ],
        "interface FastEthernet0/2": [
            "switchport mode access",
            "switchport access vlan 20",
        ],
    }

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    ignore = ["duplex", "alias", "Current configuration"]


    def ignore_command(command, ignore):
        """
        The function checks if the command contains a word from the ignore list.

        command is a string. Command to check
        ignore is a list. Word list

        Returns
        * True if the command contains a word from the ignore list
        * False - if not
        """
        ignore_status = False
        for word in ignore:
            if word in command:
                ignore_status = True
        return ignore_status
