.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst


Task 24.1
~~~~~~~~~~~~

Create a CiscoSSH class that inherits the BaseSSH class
from the base_connect_class.py file.

Create an __init__ method in the CiscoSSH class so that after connecting
via SSH, it switches to enable mode.

To do this, the __init__ method must first call the __init__ method of
the BaseSSH class, and then switch to enable mode.

.. code:: python

    In [2]: from task_24_1 import CiscoSSH

    In [3]: r1 = CiscoSSH(**device_params)

    In [4]: r1.send_show_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '


Task 24.1a
~~~~~~~~~~~~~

Copy and update the CiscoSSH class from task 24.1.

Before connecting via SSH, you need to check if the dictionary with the connection
parameters contains the following parameters: username, password, secret.
If any parameter is missing, ask the user for a value and then connect.
If all parameters are present, connect.

.. code:: python

    In [1]: from task_24_1a import CiscoSSH

    In [2]: device_params = {
       ...:         'device_type': 'cisco_ios',
       ...:         'host': '192.168.100.1',
       ...: }

    In [3]: r1 = CiscoSSH(**device_params)
    Enter username: cisco
    Enter password: cisco
    Enter enable passwod: cisco

    In [4]: r1.send_show_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '


Task 24.2
~~~~~~~~~~~~

Create a MyNetmiko class that inherits the CiscoIosSSH class from netmiko.
Write the __init__ method in the MyNetmiko class so that after connecting
via SSH, it switches to enable mode.

To do this, the __init__ method must first call the __init__ method of
the CiscoIosSSH class, and then switch to enable mode.

Check that the send_command and send_config_set methods are available
in the MyNetmiko class (they are inherited automatically, this is just for checking).

.. code:: python

    In [2]: from task_24_2 import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '


Task 24.2a
~~~~~~~~~~~~~

Copy and update the MyNetmiko class from task 24.2.

Add the _check_error_in_command method that checks for such errors:
Invalid input detected, Incomplete command, Ambiguous command

The method expects a command and command output as an argument. If no error
is found in the output, the method returns nothing. If an error is found
in the output, the method should raise an ErrorInCommand exception with a message
about which error was detected, on which device, and in which command.

An ErrorInCommand exception is created in the task file.

Rewrite send_command method to include error checking.

.. code:: python

    In [2]: from task_24_2a import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '

    In [5]: r1.send_command('sh ip br')
    ---------------------------------------------------------------------------
    ErrorInCommand                            Traceback (most recent call last)
    <ipython-input-2-1c60b31812fd> in <module>()
    ----> 1 r1.send_command('sh ip br')
    ...
    ErrorInCommand: When executing the command "sh ip br" on device 192.168.100.1, an error occurred "Invalid input detected at '^' marker."


Task 24.2b
~~~~~~~~~~~~~

Copy the class MyNetmiko from task 24.2a.

Add error checking to the send_config_set method using
the _check_error_in_command method.

The send_config_set method should send commands one at a time and check each for errors.
If no errors are encountered while executing the commands, the send_config_set method
returns the output of the commands.

.. code:: python

    In [2]: from task_24_2b import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_config_set('lo')
    ---------------------------------------------------------------------------
    ErrorInCommand                            Traceback (most recent call last)
    <ipython-input-2-8e491f78b235> in <module>()
    ----> 1 r1.send_config_set('lo')

    ...
    ErrorInCommand: When executing the command "lo" on device 192.168.100.1, an error occurred "Incomplete command."

Task 24.2c
~~~~~~~~~~~~~

Copy the class MyNetmiko from task 24.2b.
Check that the send_command method, in addition to a command,
also accepts additional arguments, for example, strip_command.

If an error occurs, rewrite the method to accept any arguments that netmiko supports.

.. code:: python

    In [2]: from task_24_2c import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_command('sh ip int br', strip_command=False)
    Out[4]: 'sh ip int br\nInterface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '

    In [5]: r1.send_command('sh ip int br', strip_command=True)
    Out[5]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '


Task 24.2d
~~~~~~~~~~~~~

Copy class MyNetmiko from task 24.2c or task 24.2b.

Add the ignore_errors parameter to the send_config_set method.
If ignore_errors=True, no error checking is needed and the method
should work exactly like the send_config_set method in netmiko.
If ignore_errors=False, errors should be checked.

By default, errors should be ignored.

.. code:: python

    In [2]: from task_24_2d import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [6]: r1.send_config_set('lo')
    Out[6]: 'config term\nEnter configuration commands, one per line.  End with CNTL/Z.\nR1(config)#lo\n% Incomplete command.\n\nR1(config)#end\nR1#'

    In [7]: r1.send_config_set('lo', ignore_errors=True)
    Out[7]: 'config term\nEnter configuration commands, one per line.  End with CNTL/Z.\nR1(config)#lo\n% Incomplete command.\n\nR1(config)#end\nR1#'

    In [8]: r1.send_config_set('lo', ignore_errors=False)
    ---------------------------------------------------------------------------
    ErrorInCommand                            Traceback (most recent call last)
    <ipython-input-8-704f2e8d1886> in <module>()
    ----> 1 r1.send_config_set('lo', ignore_errors=False)

    ...
    ErrorInCommand: When executing the command "lo" on device 192.168.100.1, an error occurred "Incomplete command."
