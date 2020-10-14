Inheritance basics
~~~~~~~~~~~~~~~~~~~

Inheritance allows creation of new classes based on existing ones. There are child and parents classes: child class inherits parent class. In inheritance, child class inherits all methods and attributes of parent class.

Example of ConnectSSH class that performs SSH connection using paramiko:

.. code:: python

    import paramiko
    import time


    class ConnectSSH:
        def __init__(self, ip, username, password):
            self.ip = ip
            self.username = username
            self.password = password
            self._MAX_READ = 10000

            client = paramiko.SSHClient()
            client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

            client.connect(
                hostname=ip,
                username=username,
                password=password,
                look_for_keys=False,
                allow_agent=False)

            self._ssh = client.invoke_shell()
            time.sleep(1)
            self._ssh.recv(self._MAX_READ)

        def __enter__(self):
            return self

        def __exit__(self, exc_type, exc_value, traceback):
            self._ssh.close()

        def close(self):
            self._ssh.close()

        def send_show_command(self, command):
            self._ssh.send(command + '\n')
            time.sleep(2)
            result = self._ssh.recv(self._MAX_READ).decode('ascii')
            return result

        def send_config_commands(self, commands):
            if isinstance(commands, str):
                commands = [commands]
            for command in commands:
                self._ssh.send(command + '\n')
                time.sleep(0.5)
            result = self._ssh.recv(self._MAX_READ).decode('ascii')
            return result

This class will be used as the basis for classes that are responsible for connecting to devices of different vendors. For example, CiscoSSH class will be responsible for connecting to Cisco devices and will inherit ConnectSSH class.

Inheritance syntax:

.. code:: python

    class CiscoSSH(ConnectSSH):
        pass

After that, all ConnectSSH methods and attributes are available in CiscoSSH class:

.. code:: python

    In [3]: r1 = CiscoSSH('192.168.100.1', 'cisco', 'cisco')

    In [4]: r1.ip
    Out[4]: '192.168.100.1'

    In [5]: r1._MAX_READ
    Out[5]: 10000

    In [6]: r1.send_show_command('sh ip int br')
    Out[6]: 'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nLoopback0                  4.4.4.4         YES NVRAM  up                    up      \r\nLoopback33                 3.3.3.3         YES manual up                    up      \r\nLoopback90                 90.1.1.1        YES manual up                    up      \r\nR1#'



    In [7]: r1.send_show_command('enable')
    Out[7]: 'enable\r\nPassword: '

    In [8]: r1.send_show_command('cisco')
    Out[8]: '\r\nR1#'

    In [9]: r1.send_config_commands(['conf t', 'int loopback 33',
       ...:                          'ip address 3.3.3.3 255.255.255.255', 'end'])
    Out[9]: 'conf t\r\nEnter configuration commands, one per line.  End with CNTL/Z.\r\nR1(config)#int loopback 33\r\nR1(config-if)#ip address 3.3.3.3 255.255.255.255\r\nR1(config-if)#end\r\nR1#'


After inheriting all methods of parent class, child class can:

* leave them unchanged
* rewrite them completely
* supplement method
* add your methods

In CiscoSSH class you have to create __init__() method and add parameters to it:

* enable_password - enable password
* disable_paging - is responsible for paging turning on/off

Method __init__() can be created entirely from scratch but basic SSH connection logic is the same in ConnectSSH and CiscoSSH, so it is better to add necessary parameters and call __init__() method of ConnectSSH class for connection. There are several options for calling parent method, for example, all of these options will call send_show_command() method of parent class from child class CiscoSSH:

.. code:: python

    command_result = ConnectSSH.send_show_command(self, command)
    command_result = super(CiscoSSH, self).send_show_command(command)
    command_result = super().send_show_command(command)

The first variant of ``ConnectSSH.send_show_command`` explicitly specifies the name of parent class - this is the most understandable variant for perception, but its disadvantage is that when a parent class name is changed the name will have to be changed in all places where parent class methods were called. This option also has disadvantages when using multiple inheritance. The second and third options are essentially equivalent but the third option is shorter, so we will use it.

CiscoSSH class with __init__() method:

.. code:: python

    class CiscoSSH(ConnectSSH):
        def __init__(self, ip, username, password, enable_password,
                     disable_paging=True):
            super().__init__(ip, username, password)
            self._ssh.send('enable\n')
            self._ssh.send(enable_password + '\n')
            if disable_paging:
                self._ssh.send('terminal length 0\n')
            time.sleep(1)
            self._ssh.recv(self._MAX_READ)

Method __init__() in CiscoSSH class added enable_password and disable_paging parameters and uses them accordingly to enter enable mode and disable paging. 
Example of connection:

.. code:: python

    In [10]: r1 = CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco')

    In [11]: r1.send_show_command('sh clock')
    Out[11]: 'sh clock\r\n*11:30:50.280 UTC Mon Aug 5 2019\r\nR1#'

Now when connecting,  switch enters enable mode and paging is disabled by default, so you can try to run a long command like sh run.

Another method that should be further developed is send_config_commands() method: since CiscoSSH class is designed to work with Cisco, you can add switching to configuration mode before commands and exit after.

.. code:: python

    class CiscoSSH(ConnectSSH):
        def __init__(self, ip, username, password, enable_password,
                     disable_paging=True):
            super().__init__(ip, username, password)
            self._ssh.send('enable\n')
            self._ssh.send(enable_password + '\n')
            if disable_paging:
                self._ssh.send('terminal length 0\n')
            time.sleep(1)
            self._ssh.recv(self._MAX_READ)

        def config_mode(self):
            self._ssh.send('conf t\n')
            time.sleep(0.5)
            result = self._ssh.recv(self._MAX_READ).decode('ascii')
            return result

        def exit_config_mode(self):
            self._ssh.send('end\n')
            time.sleep(0.5)
            result = self._ssh.recv(self._MAX_READ).decode('ascii')
            return result

        def send_config_commands(self, commands):
            result = self.config_mode()
            result += super().send_config_commands(commands)
            result += self.exit_config_mode()
            return result

Example of send_config_commands() method use:

.. code:: python

    In [12]: r1 = CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco')

    In [13]: r1.send_config_commands(['interface loopback 33',
        ...:                          'ip address 3.3.3.3 255.255.255.255'])
    Out[13]: 'conf t\r\nEnter configuration commands, one per line.  End with CNTL/Z.\r\nR1(config)#interface loopback 33\r\nR1(config-if)#ip address 3.3.3.3 255.255.255.255\r\nR1(config-if)#end\r\nR1#'

