Context manager
~~~~~~~~~~~~~~~~~~

Context manager allows specified actions to be performed at the beginning and end of *with* block. Two methods are responsible for context manager:

* ``__enter__(self)`` - indicates what should be done at the beginning of *with* block. Value that returns method is assigned to variable after *as*.
* ``__exit__(self, exc_type, exc_value, traceback)`` - indicates what should be done at the end of *with* block or when it is interrupted. If there is an exception within block, then exc_type, exc_value, traceback will contain exception information, if there is no exception they will be equal to None.

Examples of context manager usage:

* file opening/closing
* opening/closing of SSH/Telnet session
* transactions handling in database

CiscoSSH class uses paramiko to connect to the equipment:

.. code:: python

    class CiscoSSH:
        def __init__(self, ip, username, password, enable, disable_paging=True):
            client = paramiko.SSHClient()
            client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

            client.connect(
                hostname=ip,
                username=username,
                password=password,
                look_for_keys=False,
                allow_agent=False)

            self.ssh = client.invoke_shell()
            self.ssh.send('enable\n')
            self.ssh.send(enable + '\n')
            if disable_paging:
                self.ssh.send('terminal length 0\n')
            time.sleep(1)
            self.ssh.recv(1000)

        def send_show_command(self, command):
            self.ssh.send(command + '\n')
            time.sleep(2)
            result = self.ssh.recv(5000).decode('ascii')
            return result

Example of class usage:

.. code:: python


    In [9]: r1 = CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco')

    In [10]: r1.send_show_command('sh clock')
    Out[10]: 'sh clock\r\n*12:58:47.523 UTC Sun Jul 28 2019\r\nR1#'

    In [11]: r1.send_show_command('sh ip int br')
    Out[11]: 'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nLoopback0                  4.4.4.4         YES NVRAM  up                    up      \r\nLoopback90                 90.1.1.1        YES manual up                    up      \r\nR1#'


In order for the class to support work in context manager, it is necessary to add methods __enter__ and __exit__:

.. code:: python

    class CiscoSSH:
        def __init__(self, ip, username, password, enable, disable_paging=True):
            print('Метод __init__')
            client = paramiko.SSHClient()
            client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

            client.connect(
                hostname=ip,
                username=username,
                password=password,
                look_for_keys=False,
                allow_agent=False)

            self.ssh = client.invoke_shell()
            self.ssh.send('enable\n')
            self.ssh.send(enable + '\n')
            if disable_paging:
                self.ssh.send('terminal length 0\n')
            time.sleep(1)
            self.ssh.recv(1000)

        def __enter__(self):
            print('Метод __enter__')
            return self

        def __exit__(self, exc_type, exc_value, traceback):
            print('Метод __exit__')
            self.ssh.close()

        def send_show_command(self, command):
            self.ssh.send(command + '\n')
            time.sleep(2)
            result = self.ssh.recv(5000).decode('ascii')
            return result

Example of class usage in context manager:

.. code:: python

    In [14]: with CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco') as r1:
        ...:     print(r1.send_show_command('sh clock'))
        ...:
    Метод __init__
    Метод __enter__
    sh clock
    *13:05:50.677 UTC Sun Jul 28 2019
    R1#
    Метод __exit__

Even if an exception occurs within block, __exit__ method is executed:

.. code:: python

    In [18]: with CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco') as r1:
        ...:     result = r1.send_show_command('sh clock')
        ...:     result / 2
        ...:
    Метод __init__
    Метод __enter__
    Метод __exit__
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-18-b9ff1fa74be2> in <module>
          1 with CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco') as r1:
          2     result = r1.send_show_command('sh clock')
    ----> 3     result / 2
          4

    TypeError: unsupported operand type(s) for /: 'str' and 'int'


