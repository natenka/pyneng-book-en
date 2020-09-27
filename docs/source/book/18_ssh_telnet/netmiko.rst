Module netmiko
--------------

Netmiko is a module that makes it easier to use paramiko for network devices. Netmiko uses paramiko but also creates interface and methods needed to work with network devices.

Netmiko first needs to install:

::

    pip install netmiko


Supported device types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Netmiko supports several types of devices:

* Arista vEOS 
* Cisco ASA 
* Cisco IOS 
* Cisco IOS-XR 
* Cisco SG300 
* HP Comware7 
* HP ProCurve 
* Juniper Junos 
* Linux 
* and other

The whole list can be viewed in module 
`repository <https://github.com/ktbyers/netmiko>`__.

Dictionary for defining device parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dictionary may have the next parameters:

.. code:: python

    cisco_router = {'device_type': 'cisco_ios', # predefined device type
                    'ip': '192.168.1.1', # device IP address
                    'username': 'user', # username
                    'password': 'userpass', # user password
                    'secret': 'enablepass', # enable password
                    'port': 20022, # port SSH, by default 22
                     }

Connect via SSH
~~~~~~~~~~~~~~~~~~

.. code:: python

    ssh = ConnectHandler(**cisco_router)

Enable mode
~~~~~~~~~~~~

Switch to enable mode:

.. code:: python

    ssh.enable()

Exit enable mode:

.. code:: python

    ssh.exit_enable_mode()

Sending commands
~~~~~~~~~~~~~~~

Netmiko has several ways to send commands:

* ``send_command`` - send one command
* ``send_config_set`` - send list of commands or command in configuration mode 
* ``send_config_from_file`` - send commands from the file (uses  ``send_config_set``  method inside)
* ``send_command_timing`` - send command and wait for the output based on timer

``send_command``
^^^^^^^^^^^^^^^^

Method send_command allows you to send one command to device.

For example:

.. code:: python

    result = ssh.send_command('show ip int br')

The method works as follows:

* sends command to device and gets the output until the string with prompt or until the specified string

  * prompt is automatically determined
  * if your device does not determine it, you can simply specify a string till which to read the output
  * ``send_command_expect`` method previously worked this way, but since version 1.0.0 this is how send_command works and send_command_expect method is left for compatibility

* method returns command output 
* the following parameters can be passed to method:

  * ``command_string`` - command 
  * ``expect_string`` - till which string read output
  * ``delay_factor`` - option allows to increase delay before the start of string search
  * ``max_loops`` - number of iterations before method gives out an error (exception). By default 500 
  * ``strip_prompt`` - remove prompt from the output. Removed by default
  * ``strip_command`` - remove command from output

In most cases, only command will be sufficient to specify.

``send_config_set``
*******************

Method ``send_config_set`` allows you to send command or multiple commands in configuration mode.

Example of use:

.. code:: python

    commands = ['router ospf 1',
                'network 10.0.0.0 0.255.255.255 area 0',
                'network 192.168.100.0 0.0.0.255 area 1']

    result = ssh.send_config_set(commands)

Method works as follows:

* goes into configuration mode, 
* then passes all commands
* and exits configuration mode
* depending on device type, there may be no exit from configuration mode. For example, there will be no exit for IOS-XR because you first have to commit changes

``send_config_from_file``
^^^^^^^^^^^^^^^^^^^^^^^^^

Method ``send_config_from_file`` sends commands from specified file to configuration mode.

Example of use:

.. code:: python

    result = ssh.send_config_from_file('config_ospf.txt')

Method opens a file, reads commands and passes them to 
``send_config_set`` method.

Additional methods
~~~~~~~~~~~~~~~~~~~~~

Besides the above methods for sending commands, netmiko supports such methods:

* ``config_mode`` - switch to configuration mode: ``ssh.config_mode()`` 
* ``exit_config_mode`` - exit configuration mode: ``ssh.exit_config_mode()`` 
* ``check_config_mode`` - check whether netmiko is in configuration mode (returns True if in configuration mode and False if not): ``ssh.check_config_mode()`` 
* ``find_prompt`` - returns the current prompt of device: ``ssh.find_prompt()`` 
* ``commit`` - commit on IOS-XR and Juniper: ``ssh.commit()`` 
* ``disconnect`` - terminate SSH connection

.. note::

    Above ssh is a pre-created SSH connection:
    ``ssh = ConnectHandler(**cisco_router)``

Telnet support
~~~~~~~~~~~~~~~~

Since version 1.0.0 netmiko supports Telnet connections, so far only for Cisco IOS devices.

Inside netmiko uses telnetlib to connect via Telnet. But, at the same time, it provides the same interface for work as for SSH connection.

In order to connect via Telnet, it is sufficient in the dictionary that defines connection parameters specify device type 'cisco_ios_telnet':

.. code:: python

    device = {
        "device_type": "cisco_ios_telnet",
        "ip": "192.168.100.1",
        "username": "cisco",
        "password": "cisco",
        "secret": "cisco",
    }

Otherwise, methods that apply to SSH apply to Telnet. An example similar to SSH (4_netmiko_telnet.py file):

.. code:: python

    from pprint import pprint
    import yaml
    from netmiko import (
        ConnectHandler,
        NetmikoTimeoutException,
        NetmikoAuthenticationException,
    )


    def send_show_command(device, commands):
        result = {}
        try:
            with ConnectHandler(**device) as ssh:
                ssh.enable()
                for command in commands:
                    output = ssh.send_command(command)
                    result[command] = output
            return result
        except (NetmikoTimeoutException, NetmikoAuthenticationException) as error:
            print(error)


    if __name__ == "__main__":
        device = {
            "device_type": "cisco_ios_telnet",
            "ip": "192.168.100.1",
            "username": "cisco",
            "password": "cisco",
            "secret": "cisco",
        }
        result = send_show_command(device, ["sh clock", "sh ip int br"])
        pprint(result, width=120)



Other methods works similarly: 

* ``send_command_timing()`` 
* ``find_prompt()`` 
* ``send_config_set()`` 
* ``send_config_from_file()`` 
* ``check_enable_mode()`` 
* ``disconnect()``


Example of netmiko use
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example of netmiko use (4_netmiko.py file):

.. code:: python

    from pprint import pprint
    import yaml
    from netmiko import (
        ConnectHandler,
        NetmikoTimeoutException,
        NetmikoAuthenticationException,
    )


    def send_show_command(device, commands):
        result = {}
        try:
            with ConnectHandler(**device) as ssh:
                ssh.enable()
                for command in commands:
                    output = ssh.send_command(command)
                    result[command] = output
            return result
        except (NetmikoTimeoutException, NetmikoAuthenticationException) as error:
            print(error)


    if __name__ == "__main__":
        with open("devices.yaml") as f:
            devices = yaml.safe_load(f)
        for device in devices:
            result = send_show_command(device, ["sh clock", "sh ip int br"])
            pprint(result, width=120)



In this example *terminal length* command is not passed because netmiko executes this command by default.

The result of script execution:

::

    {'sh clock': '*09:12:15.210 UTC Mon Jul 20 2020',
     'sh ip int br': 'Interface     IP-Address      OK? Method Status                Protocol\n'
                     'Ethernet0/0   192.168.100.1   YES NVRAM  up                    up      \n'
                     'Ethernet0/1   192.168.200.1   YES NVRAM  up                    up      \n'
                     'Ethernet0/2   unassigned      YES NVRAM  up                    up      \n'
                     'Ethernet0/3   192.168.130.1   YES NVRAM  up                    up      \n'}
    {'sh clock': '*09:12:24.507 UTC Mon Jul 20 2020',
     'sh ip int br': 'Interface     IP-Address      OK? Method Status                Protocol\n'
                     'Ethernet0/0   192.168.100.2   YES NVRAM  up                    up      \n'
                     'Ethernet0/1   unassigned      YES NVRAM  up                    up      \n'
                     'Ethernet0/2   unassigned      YES NVRAM  administratively down down    \n'
                     'Ethernet0/3   unassigned      YES NVRAM  administratively down down    \n'}
    {'sh clock': '*09:12:33.573 UTC Mon Jul 20 2020',
     'sh ip int br': 'Interface     IP-Address      OK? Method Status                Protocol\n'
                     'Ethernet0/0   192.168.100.3   YES NVRAM  up                    up      \n'
                     'Ethernet0/1   unassigned      YES NVRAM  up                    up      \n'
                     'Ethernet0/2   unassigned      YES NVRAM  administratively down down    \n'
                     'Ethernet0/3   unassigned      YES NVRAM  administratively down down    \n'}


Paginated command output
~~~~~~~~~~~~~~~~~~~~~~~~~

Example of using netmiko with paginated output of *show* command (4_netmiko_more.py file):

.. code:: python

    from netmiko import ConnectHandler, NetmikoTimeoutException
    import yaml


    def send_show_command(device_params, command):
        with ConnectHandler(**device_params) as ssh:
            ssh.enable()
            prompt = ssh.find_prompt()
            ssh.send_command("terminal length 100")
            ssh.write_channel(f"{command}\n")
            output = ""
            while True:
                try:
                    page = ssh.read_until_pattern(f"More|{prompt}")
                    output += page
                    if "More" in page:
                        ssh.write_channel(" ")
                    elif prompt in output:
                        break
                except NetmikoTimeoutException:
                    break
        return output


    if __name__ == "__main__":
        with open("devices.yaml") as f:
            devices = yaml.safe_load(f)
        print(send_show_command(devices[0], "sh run"))

