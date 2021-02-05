.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 18.1
~~~~~~~~~~~~

Create send_show_command() function.

Function connects via SSH (using netmiko) to one device and performs specified command.

Function parameters:

* device - dictionary with device connection parameters
* command - command to execute

Function returns a string with command output.

Script should send *command* command to all devices from device.yaml file using send_show_command() function.

.. code:: python

    command = "sh ip int br"

Task 18.1a
~~~~~~~~~~~~~

Copy send_show_command() function from task 18.1 and redo it to process the exception that is generated when authentication on device fails.

If error occurs, exception message should be displayed on standard output stream.

To verify this, change your password on device or in devices.yaml

Task 18.1b
~~~~~~~~~~~~~

Copy send_show_command() function from task 18.1a and redo it in such a way that exception is generated not only when authentication on device fails, but also when device's IP address is not available.

If error occurs, exception message should be displayed on standard output stream.

To verify this, change your password on device or in devices.yaml

Task 18.2
~~~~~~~~~~~~

Create send_config_commands() function

Function connects via SSH (using netmiko) to device and performs a list of commands in configuration mode based on passed arguments.

Function parameters:

* device - dictionary with device connection parameters
* config_commands - list of commands to execute

Function returns a string with command output.

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


Script should send *command* command to all devices from device.yaml file using send_config_commands() function.

.. code:: python

    commands = [
        'logging 10.255.255.1', 'logging buffered 20010', 'no logging console'
    ]

Task 18.2a
~~~~~~~~~~~~~

Copy send_config_commands() function from task 18.2 and add *verbose* parameter that controls whether information about to which device connection is established will be displayed in output.

.. note::
    verbose - parameter of send_config_commands() function, not parameter of ConnectHandler!

By default, the result should be displayed.

Example of function execution:

.. code:: python

    In [13]: result = send_config_commands(r1, commands)
    Connection to 192.168.100.1...

    In [14]: result = send_config_commands(r1, commands, verbose=False)

    In [15]:

Script should send commands list to all devices from devices.yaml file using the send_config_commands() function.


Task 18.2b
~~~~~~~~~~~~~

Copy send_config_commands() function from task 18.2a and add error check.

When executing each command, script should check the result for such errors:

* Invalid input detected, Incomplete command, Ambiguous command

If error occurs during execution of any of commands, function should output a message to standard output stream with information about: which error occurred, which command caused it and on which device. For example: "logging" command was executed with error "Incomplete command." on device 192.168.100.1

Errors should always be displayed regardless of *verbose* parameter value. However, *verbose* still has to control whether the message will be displayed:
Connecting to 192.168.100.1...

Function send_config_commands() should now return a tuple with two dictionaries:

* first dictionary with commands output that executed without error
* second dictionary with commands output that executed with errors

Both dictionaries in format:

* key - command
* value - output with execution of commands

Function can be checked on one device.


Example of send_config_commands() function execution:

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

Copy send_config_commands() function from 18.2b task and redo it in the following way: If you have error when executing a command, ask user if you need to execute other commands.

Response options [y]/n:

* y - to execute other commands. This is the default, so pressing any combination is perceived as "y"
* n or no - do not execute other commands

Function send_config_commands() should still return a tuple with two dictionaries:

* first dictionary with commands output that executed without error
* second dictionary with commands output that executed with errors

Both dictionaries in format:

* key - command
* value - output with execution of commands

Function can be checked on one device.

Example of function execution:

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

Create send_commands() function (netmiko is used to connect via SSH).

Function parameters:

* device - dictionary with device connection parameters
* show - one show command (string)
* config - list of commands to execute in configuration mode

Depending on which argument is passed, the function calls different functions within. When calling send_commands(), only one argumnet will be passed - show or config.

Then follows the combination of argument and corresponding fucntion:

* show - send_show_command() function from task 18.1
* config - send_config_commands() function from task 18.2

Function returns string with execution results of command or commands.

Check function with:

* *commands* - list of commands
* *command* - command

Example of function execution:


.. code:: python

    In [14]: send_commands(r1, show='sh clock')
    Out[14]: '*17:06:12.278 UTC Wed Mar 13 2019'

    In [15]: send_commands(r1, config=['username user5 password pass5', 'username user6 password pass6'])
    Out[15]: 'config term\nEnter configuration commands, one per line.  End with CNTL/Z.\nR1(config)#username user5 password pass5\nR1(config)#username user6 password pass6\nR1(config)#end\nR1#'

Commands example:

.. code:: python

    commands = [
        'logging 10.255.255.1', 'logging buffered 20010']
    command = 'sh ip int br'
