.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst


Task 24.1
~~~~~~~~~~~~

Create CiscoSSH class that inherits BaseSSH class from base_connect_class.py.

Create __init__() method in CiscoSSH class in such a way that once connected by SSH, enable mode is activated.

To do this, __init__() method should first invoke __init__() method of ConnectSSH class and then switch to enable mode.

.. code-block:: python

    In [2]: from task_24_1 import CiscoSSH

    In [3]: r1 = CiscoSSH(**device_params)

    In [4]: r1.send_show_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '


Task 24.1a
~~~~~~~~~~~~~

Add CiscoSSH class from task 24.1.

Before connecting via SSH, you should check whether dictionary with parameters has:  username, password, secret. If not, request them from user and then establish connection. If parameters exist, establish connection immediately.

.. code-block:: python

    In [1]: from task_24_1a import CiscoSSH

    In [2]: device_params = {
       ...:         'device_type': 'cisco_ios',
       ...:         'ip': '192.168.100.1',
       ...: }

    In [3]: r1 = CiscoSSH(**device_params)
    Enter user name: cisco
    Enter password:
    Enter password for enable mode:

    In [4]: r1.send_show_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '

Task 24.2
~~~~~~~~~~~~

Create MyNetmiko class that inherits CiscoIosBase class from netmiko.

Rewrite __init__() method in MyNetmiko class in such a way that once connected via SSH, enable mode is activated.

To do this, __init__() method should first call __init__() method of CiscoIosBase class and then switch to enable mode.

Check that send_command() and send_config_set() methods are available in MyNetmiko class

.. code-block:: python

    In [2]: from task_24_2 import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_command('sh ip int br')
    Out[4]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '

CiscoIosSSH class import:

.. code:: python

    from netmiko.cisco.cisco_ios import CiscoIosSSH


    device_params = {
        "device_type": "cisco_ios",
        "ip": "192.168.100.1",
        "username": "cisco",
        "password": "cisco",
        "secret": "cisco",
    }

Task 24.2a
~~~~~~~~~~~~~

Complete MyNetmikoclass from task 24.2.

Add _check_error_in_command() method that checks for such errors:

* Invalid input detected
* Incomplete command
* Ambiguous command

Method expects as an argument the command and output of command. If no error was found in output, method does not return anything. If error is found in output, method generates an ErrorInCommand exception with message about what error was detected, on which device and on which command.

Rewrite send_command netmiko() method by adding error check.

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
    ErrorInCommand: When executing command "sh ip br" on device 192.168.100.1 error occurred "Invalid input detected at '^' marker."

ErrorInCommand exception:

.. code:: python

    class ErrorInCommand(Exception):
        """
        Exception is generated if error occurs while executing command on device.
        """


Task 24.2b
~~~~~~~~~~~~~

Copy MyNetmiko class from task 24.2a.

Complete send_config_set netmiko() method functionality and add error check using _check_error_in_command() method.

Method send_config_set() should send commands one at a time and check each for errors. If no errors are detected while executing commands, send_config_set() method returns output of commands.

.. code:: python

    In [2]: from task_24_2b import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_config_set('lo')
    ---------------------------------------------------------------------------
    ErrorInCommand                            Traceback (most recent call last)
    <ipython-input-2-8e491f78b235> in <module>()
    ----> 1 r1.send_config_set('lo')
    ...
    ErrorInCommand: When executing command "lo" on device 192.168.100.1 error occurred "Incomplete command."

Задание 24.2c
~~~~~~~~~~~~~

Check that send_command() method of MyNetmiko class from task 24.2b accepts additional arguments (as in netmiko), except command.

If error occurs, redo method so that it accepts any arguments that support netmiko.

.. code:: python

    In [2]: from task_24_2c import MyNetmiko

    In [3]: r1 = MyNetmiko(**device_params)

    In [4]: r1.send_command('sh ip int br', strip_command=False)
    Out[4]: 'sh ip int br\nInterface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '

    In [5]: r1.send_command('sh ip int br', strip_command=True)
    Out[5]: 'Interface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      '

Task 24.2d
~~~~~~~~~~~~~

Copy MyNetmiko class from task 24.2c or 24.2b.

Add ignore_errors parameter to send_config_set() method. If true value is passed, no error check should be performed and method should run in the same way as send_config_set() method in netmiko. If value is false, errors should be checked.

Errors should be ignored by default.

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
    ErrorInCommand: When executing command "lo" on device 192.168.100.1 error occurred "Incomplete command."
