.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 18.1
~~~~~~~~~~~~

Create send_show_command function.

The function connects via SSH (using netmiko) to ONE device and executes
the specified command.

Function parameters:

* device - a dictionary with parameters for connecting to a device
* command - the command to be executed

The function should return a string with the command output.

The script should send command command to all devices from the devices.yaml file
using the send_show_command function (this part of the code is written).

.. code:: python

    import yaml


    if __name__ == "__main__":
        command = "sh ip int br"
        with open("devices.yaml") as f:
            devices = yaml.safe_load(f)

        for dev in devices:
            print(send_show_command(dev, command))

Task 18.1a
~~~~~~~~~~

Copy the send_show_command function from task 18.1 and rewrite it to handle
the exception that is thrown on authentication failure on the device.

When an error occurs, an exception message should be printed to stdout.

To verify, change the password on the device or in the devices.yaml file.

Task 18.1b
~~~~~~~~~~~~~

Copy the send_show_command function from task 18.1a and rewrite it to handle
not only the exception that is raised when authentication fails on the device,
but also the exception that is raised when the IP address of the device
is not available.

When an error occurs, an exception message should be printed to standard output.

To check, change the IP address on the device or in the devices.yaml file.

Task 18.2
~~~~~~~~~~~~

Create send_config_commands function

The function connects via SSH (using netmiko) to ONE device and executes
a list of commands in configuration mode based on the passed arguments.

Function parameters:

* device - a dictionary with parameters for connecting to a device
* config_commands - list of configuration commands to be executed

The function should return a string with the results of the command:

.. code:: python

    In [7]: r1
    Out[7]:
    {'device_type': 'cisco_ios',
     'ip': '192.168.100.1',
     'username': 'cisco',
     'password': 'cisco',
     'secret': 'cisco'}

    In [8]: commands
    Out[8]: ['logging 10.255.255.1', 'logging buffered 20010', 'no logging console']

    In [9]: result = send_config_commands(r1, commands)

    In [10]: result
    Out[10]: 'config term\nEnter configuration commands, one per line.  End with CNTL/Z.\nR1(config)#logging 10.255.255.1\nR1(config)#logging buffered 20010\nR1(config)#no logging console\nR1(config)#end\nR1#'

    In [11]: print(result)
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#logging 10.255.255.1
    R1(config)#logging buffered 20010
    R1(config)#no logging console
    R1(config)#end
    R1#

The script should send command command to all devices from the devices.yaml file
using the send_config_commands function.

.. code:: python

    commands = [
        'logging 10.255.255.1', 'logging buffered 20010', 'no logging console'
    ]

Task 18.2a
~~~~~~~~~~~~~

Copy the send_config_commands function from job 18.2 and add the log parameter.
The log parameter controls whether information is displayed about which device
the connection is to:

* if log is equal to True - information is printed
* if log is equal to False - information is not printed

By default, log is equal to True.

An example of how the function works:

.. code:: python

    In [13]: result = send_config_commands(r1, commands)
    Connecting to 192.168.100.1...

    In [14]: result = send_config_commands(r1, commands, log=False)

    In [15]:

The script should send command command to all devices from the devices.yaml file
using the send_config_commands function.

Task 18.2b
~~~~~~~~~~~~~

Copy the send_config_commands function from task 18.2a and add error checking.

When executing each command, the script should check the output for
the following errors: Invalid input detected, Incomplete command, Ambiguous command


If an error occurs while executing any of the commands, the function should
print a message to the stdout with information about what error occurred,
in which command and on which device, for example:
The "logging" command was executed with the error "Incomplete command." on the device 192.168.100.1

Errors should always be printed, regardless of the value of the log parameter.
At the same time, the log parameter should still control whether the message
"Connecting to 192.168.100.1..." will be displayed.

Send_config_commands should now return a tuple of two dictionaries:

* the first dict with the output of commands that were executed without error
* second dict with the output of commands that were executed with errors

In both dictionaries:

* key - command
* value - output with command execution

You can test the function on one device.

An example of how the send_config_commands function works:

.. code:: python

    In [16]: commands
    Out[16]:
    ['logging 0255.255.1',
     'logging',
     'a',
     'logging buffered 20010',
     'ip http server']

    In [17]: result = send_config_commands(r1, commands)
    Connecting to 192.168.100.1...
    "logging 0255.255.1" command was executed with error "Invalid input detected at '^' marker." on device 192.168.100.1
    "logging" command was executed with error "Incomplete command." on device 192.168.100.1
    "a" command was executed with error "Ambiguous command:  "a"" on device 192.168.100.1

    In [18]: pprint(result, width=120)
    ({'ip http server': 'config term\n'
                        'Enter configuration commands, one per line.  End with CNTL/Z.\n'
                        'R1(config)#ip http server\n'
                        'R1(config)#',
      'logging buffered 20010': 'config term\n'
                                'Enter configuration commands, one per line.  End with CNTL/Z.\n'
                                'R1(config)#logging buffered 20010\n'
                                'R1(config)#'},
     {'a': 'config term\n'
           'Enter configuration commands, one per line.  End with CNTL/Z.\n'
           'R1(config)#a\n'
           '% Ambiguous command:  "a"\n'
           'R1(config)#',
      'logging': 'config term\n'
                 'Enter configuration commands, one per line.  End with CNTL/Z.\n'
                 'R1(config)#logging\n'
                 '% Incomplete command.\n'
                 '\n'
                 'R1(config)#',
      'logging 0255.255.1': 'config term\n'
                            'Enter configuration commands, one per line.  End with CNTL/Z.\n'
                            'R1(config)#logging 0255.255.1\n'
                            '                   ^\n'
                            "% Invalid input detected at '^' marker.\n"
                            '\n'
                            'R1(config)#'})

    In [19]: good, bad = result

    In [20]: good.keys()
    Out[20]: dict_keys(['logging buffered 20010', 'ip http server'])

    In [21]: bad.keys()
    Out[21]: dict_keys(['logging 0255.255.1', 'logging', 'a'])


Examples of commands with errors:

::

    R1(config)#logging 0255.255.1
                       ^
    % Invalid input detected at '^' marker.

    R1(config)#logging
    % Incomplete command.

    R1(config)#a
    % Ambiguous command:  "a"


Lists of command lists with and without errors:

.. code:: python

    commands_with_errors = ['logging 0255.255.1', 'logging', 'a']
    correct_commands = ['logging buffered 20010', 'ip http server']

    commands = commands_with_errors + correct_commands

Task 18.2c
~~~~~~~~~~~~~

Copy the send_config_commands function from task 18.2b and remake it as follows:
If an error occurs while executing a command, ask the user whether to continue
executing other commands.

Answer options [y]/n:

* y - execute other commands. This is the default, so any key is interpreted as y
* n or no - do not execute other commands

The send_config_commands function should still return a tuple of two dictionaries:

* the first dictionary with the output of commands that were executed without error
* second dictionary with the output of commands that were executed with errors

In both dictionaries:

* key - command
* value - output with command execution

You can test the function on one device.

An example of how the send_config_commands function works:

.. code:: python

    In [11]: result = send_config_commands(r1, commands)
    Connecting to 192.168.100.1...
    "logging 0255.255.1" command was executed with error "Invalid input detected at '^' marker." on device 192.168.100.1
    Continue commands execution? [y]/n: y
    "logging" command was executed with error "Incomplete command." on device 192.168.100.1
    Continue commands execution? [y]/n: n

    In [12]: pprint(result)
    ({},
     {'logging': 'config term\n'
                 'Enter configuration commands, one per line.  End with CNTL/Z.\n'
                 'R1(config)#logging\n'
                 '% Incomplete command.\n'
                 '\n'
                 'R1(config)#',
      'logging 0255.255.1': 'config term\n'
                            'Enter configuration commands, one per line.  End with '
                            'CNTL/Z.\n'
                            'R1(config)#logging 0255.255.1\n'
                            '                   ^\n'
                            "% Invalid input detected at '^' marker.\n"
                            '\n'
                            'R1(config)#'})

Lists of commands with and without errors:

.. code:: python

    commands_with_errors = ['logging 0255.255.1', 'logging', 'a']
    correct_commands = ['logging buffered 20010', 'ip http server']

    commands = commands_with_errors + correct_commands

Task 18.3
~~~~~~~~~~~~

Create a send_commands function (use netmiko to connect via SSH).

Function parameters:

* device - a dictionary with parameters for connecting to one device
* show - one show command (string)
* config - a list with commands to be executed in configuration mode

The show and config arguments should only be passed as keyword arguments.
Passing these arguments as positional should raise a TypeError exception.

.. code:: python

    In [4]: send_commands(r1, 'sh clock')
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-4-75adcfb4a005> in <module>
    ----> 1 send_commands(r1, 'sh clock')

    TypeError: send_commands() takes 1 positional argument but 2 were given

Depending on which argument was passed, the send_commands function calls
different functions internally. When calling the send_commands function,
only one of the show, config arguments should always be passed. If both
arguments are passed, a ValueError exception should be raised.

A combination of an argument and a corresponding function:

* show - the send_show_command function from task 18.1
* config - send_config_commands function from task 18.2

The function returns a string with the results of executing single command
or multiple commands.

Check function operation:

* with a list of config commands in variable commands
* single show command in variable command

An example of how the function works:

.. code:: python

    In [14]: send_commands(r1, show='sh clock')
    Out[14]: '*17:06:12.278 UTC Wed Mar 13 2019'

    In [15]: send_commands(r1, config=['username user5 password pass5', 'username user6 password pass6'])
    Out[15]: 'config term\nEnter configuration commands, one per line.  End with CNTL/Z.\nR1(config)#username user5 password pass5\nR1(config)#username user6 password pass6\nR1(config)#end\nR1#'

Commands example:

.. code:: python

    commands = ['logging 10.255.255.1', 'logging buffered 20010']
    command = 'sh ip int br'
