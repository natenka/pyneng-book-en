Module scrapli
--------------

`scrapli <https://github.com/carlmontanari/scrapli>`__ is a module that allows
you to connect to network equipment using Telnet, SSH or NETCONF.

Just like netmiko, scrapli can use paramiko or telnetlib (and other modules)
for the connection itself, but it provides the same interface for different
types of connections and different equipment.

Installing scrapli:

::

    pip install scrapli


.. note::

    Book covers scrapli version 2021.1.30.


The three main components of scrapli are:

* transport is a specific way to connect to equipment
* channel - the next level above the transport, which is responsible for sending
  commands, receiving output and other interactions with equipment
* driver is the interface for working with scrapli. There are both specific
  drivers, for example, IOSXEDriver, which understands
  how to interact with a specific type of equipment, and the basic Driver,
  which provides a minimal interface for working via SSH/Telnet.


Available transport options:

* system - the built-in SSH client, it is assumed that the client is used on Linux/MacOS
* paramiko - the paramiko module
* ssh2 - the ssh2-python module (wrapper around the C library libssh2)
* telnet - telnetlib
* asyncssh - asyncssh module
* asynctelnet - async telnet client

Most of the examples will be using the system transport. Since the module
interface is the same for all synchronous transport options, to use a different
transport, you just need to specify it (for telnet transport, you must also
specify the port).

.. note::

    Asynchronous transport options (asyncssh, asynctelnet) are covered in the
    `Advanced Python for network engineers (russian) <https://advpyneng.readthedocs.io/ru/latest/book/17_async_libraries/scrapli.html>`__


Supported platforms:

* Cisco IOS-XE
* Cisco NX-OS
* Juniper JunOS
* Cisco IOS-XR
* Arista EOS

In addition to these platforms, there are also
`scrapli community platforms <https://github.com/scrapli/scrapli_community>`__.
And one of the advantages of scrapli is that it is relatively easy to add new platforms.

There are two connection options in scrapli: using the general Scrapli class,
which selects the required driver by the platform parameter, or a specific driver,
for example, IOSXEDriver. The same parameters are passed to the specific driver and Scrapli.

.. note::

    In addition to these options, there are also generic (base) drivers.


If the scrapli (or scrapli community) does not support the required platform,
you can add the platform to the
`scrapli community <https://github.com/scrapli/scrapli_community#adding-a-platform>`__
or use generic drivers (not covered in the book):

* `Driver <https://carlmontanari.github.io/scrapli/user_guide/advanced_usage/#using-driver-directly>`__
* `GenericDriver <https://carlmontanari.github.io/scrapli/user_guide/advanced_usage/#using-the-genericdriver>`__
* `NetworkDriver <https://carlmontanari.github.io/scrapli/user_guide/advanced_usage/>`__

Connection parameters
~~~~~~~~~~~~~~~~~~~~~

Basic connection parameters:

* host - IP address or hostname
* auth_username - username for authentication
* auth_password - password for authentication
* auth_secondary - enable password
* auth_strict_key - strict key checking (True by default)
* platform - must be specified when using Scrapli
* transport - which transport to use
* transport_options - options for a specific transport

The connection process is slightly different depending on whether you are using
a context manager or not. When connecting without a context manager, you first
need to pass parameters to the driver or Scrapli, and then call the open method:

.. code:: python

    from scrapli import Scrapli

    r1 = {
       "host": "192.168.100.1",
       "auth_username": "cisco",
       "auth_password": "cisco",
       "auth_secondary": "cisco",
       "auth_strict_key": False,
       "platform": "cisco_iosxe"
    }

    In [2]: ssh = Scrapli(**r1)

    In [3]: ssh.open()

After that, you can send commands:

.. code:: python

    In [4]: ssh.get_prompt()
    Out[4]: 'R1#'

    In [5]: ssh.close()

When using a context manager, you don't need to call open:

.. code:: python

    In [8]: with Scrapli(**r1_driver) as ssh:
       ...:     print(ssh.get_prompt())
       ...:
    R1#

Using the driver
~~~~~~~~~~~~~~~~

Available drivers

+------------------+--------------+-------------------+
| Network equipment| Драйвер      | Параметр platform |
+==================+==============+===================+
| Cisco IOS-XE     | IOSXEDriver  | cisco_iosxe       |
+------------------+--------------+-------------------+
| Cisco NX-OS      | NXOSDriver   | cisco_nxos        |
+------------------+--------------+-------------------+
| Cisco IOS-XR     | IOSXRDriver  | cisco_iosxr       |
+------------------+--------------+-------------------+
| Arista EOS       | EOSDriver    | arista_eos        |
+------------------+--------------+-------------------+
| Juniper JunOS    | JunosDriver  | juniper_junos     |
+------------------+--------------+-------------------+

Example of connection using the IOSXEDriver driver (connecting to Cisco IOS):

.. code:: python

    In [11]: from scrapli.driver.core import IOSXEDriver

    In [12]: r1_driver = {
        ...:    "host": "192.168.100.1",
        ...:    "auth_username": "cisco",
        ...:    "auth_password": "cisco",
        ...:    "auth_secondary": "cisco",
        ...:    "auth_strict_key": False,
        ...: }

    In [13]: with IOSXEDriver(**r1_driver) as ssh:
        ...:     print(ssh.get_prompt())
        ...:
    R1#

Sending commands
~~~~~~~~~~~~~~~~

Scrapli has several methods for sending commands:

* ``send_command`` - send one show command
* ``send_commands`` - send a list of show commands
* ``send_commands_from_file`` - send show commands from a file
* ``send_config`` - send one command in configuration mode
* ``send_configs`` - send a list of commands in configuration mode
* ``send_configs_from_file`` - send commands from file in configuration mode

All of these methods return a Response object, not the output of the command as a string.


Response object
~~~~~~~~~~~~~~~

The send_command method and other methods for sending commands return a Response
object (not the output of the command). Response allows you to get not only the
output of the command, but also such things as the execution time of the command,
whether there were errors during the execution of the command, structured output
using textfsm, and so on.

.. code:: python

    In [15]: reply = ssh.send_command("sh clock")

    In [16]: reply
    Out[16]: Response <Success: True>

You can get the output of the command by accessing the result attribute:

.. code:: python

    In [17]: reply.result
    Out[17]: '*17:31:54.232 UTC Wed Mar 31 2021'

The raw_result attribute contains a byte string with complete output:

.. code:: python

    In [18]: reply.raw_result
    Out[18]: b'\n*17:31:54.232 UTC Wed Mar 31 2021\nR1#'

For commands that take longer than normal show, it may be necessary
to know the command execution time:

.. code:: python

    In [18]: r = ssh.send_command("ping 10.1.1.1")

    In [19]: r.result
    Out[19]: 'Type escape sequence to abort.\nSending 5, 100-byte ICMP Echos to 10.1.1.1, timeout is 2 seconds:\n.....\nSuccess rate is 0 percent (0/5)'

    In [20]: r.elapsed_time
    Out[20]: 10.047594

    In [21]: r.start_time
    Out[21]: datetime.datetime(2021, 4, 1, 7, 10, 56, 63697)

    In [22]: r.finish_time
    Out[22]: datetime.datetime(2021, 4, 1, 7, 11, 6, 111291)

The channel_input attribute returns the command that was sent to the equipment:

.. code:: python

    In [23]: r.channel_input
    Out[23]: 'ping 10.1.1.1'


send_command method
~~~~~~~~~~~~~~~~~~~

The ``send_command`` method allows you to send one command to a device.

.. code:: python

    In [14]: reply = ssh.send_command("sh clock")

Method parameters (all these parameters must be passed as keyword arguments):

* ``strip_prompt`` - remove a prompt from the output. Deleted by default
* ``failed_when_contains`` - if the output contains the specified line or one of
  the lines in the list, the command will be considered as completed with an error
* ``timeout_ops`` - maximum time to execute a command, by default it is 30 seconds
  for IOSXEDriver

An example of using the ``send_command`` method:

.. code:: python

    In [15]: reply = ssh.send_command("sh clock")

    In [16]: reply
    Out[16]: Response <Success: True>

The timeout_ops parameter specifies how long to wait for the command to execute:

.. code:: python

    In [19]: ssh.send_command("ping 8.8.8.8", timeout_ops=20)
    Out[19]: Response <Success: True>

If the command does not complete within the specified time, a ScrapliTimeout
exception will be raised (output is truncated):

.. code:: python

    In [20]: ssh.send_command("ping 8.8.8.8", timeout_ops=2)
    ---------------------------------------------------------------------------
    ScrapliTimeout                            Traceback (most recent call last)
    <ipython-input-20-e062fb19f0e6> in <module>
    ----> 1 ssh.send_command("ping 8.8.8.8", timeout_ops=2)

In addition to receiving normal command output, scrapli also allows you to
receive structured output, for example using the textfsm_parse_output method:

.. code:: python

    In [21]: reply = ssh.send_command("sh ip int br")

    In [22]: reply.textfsm_parse_output()
    Out[22]:
    [{'intf': 'Ethernet0/0',
      'ipaddr': '192.168.100.1',
      'status': 'up',
      'proto': 'up'},
     {'intf': 'Ethernet0/1',
      'ipaddr': '192.168.200.1',
      'status': 'up',
      'proto': 'up'},
     {'intf': 'Ethernet0/2',
      'ipaddr': 'unassigned',
      'status': 'up',
      'proto': 'up'},
     {'intf': 'Ethernet0/3',
      'ipaddr': '192.168.130.1',
      'status': 'up',
      'proto': 'up'}]

.. note::

    What is TextFSM and how to work with it is covered in chapter 21. Scrapli
    uses ready-made templates in order to receive structured output and in basic
    cases does not require knowledge of TextFSM.

Error detection
~~~~~~~~~~~~~~~

Methods for sending commands automatically check the output for errors. For
each vendor/type of equipment, these are different errors,
plus you can specify which lines in the output will be considered an error.
By default, IOSXEDriver will consider the following lines as errors:

.. code:: python

    In [21]: ssh.failed_when_contains
    Out[21]:
    ['% Ambiguous command',
     '% Incomplete command',
     '% Invalid input detected',
     '% Unknown command']

The ``failed`` attribute of the Response object returns ``False`` if the command finished
without error and ``True`` if it failed.

.. code:: python

    In [23]: reply = ssh.send_command("sh clck")

    In [24]: reply.result
    Out[24]: "        ^\n% Invalid input detected at '^' marker."

    In [25]: reply
    Out[25]: Response <Success: False>

    In [26]: reply.failed
    Out[26]: True


send_config method
~~~~~~~~~~~~~~~~~~

The ``send_config`` method allows you to send one configuration mode command.

Example:

.. code:: python

    In [33]: r = ssh.send_config("username user1 password password1")

Since scrapli removes the command from the output, by default, when using
``send_config``, the result attribute will contain an empty string (if there was
no error while executing the command):

.. code:: python

    In [34]: r.result
    Out[34]: ''

You can add the parameter ``strip_prompt=False`` and then the prompt will
appear in the output:

.. code:: python

    In [37]: r = ssh.send_config("username user1 password password1", strip_prompt=False)

    In [38]: r.result
    Out[38]: 'R1(config)#'


send_commands, send_configs
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The send_commands, send_configs methods differ from send_command, send_config
in that they can send several commands. In addition, these methods do not return
a Response, but a MultiResponse object, which can generally be thought of as a list
of Response objects, one for each command.

.. code:: python

    In [44]: reply = ssh.send_commands(["sh clock", "sh ip int br"])

    In [45]: reply
    Out[45]: MultiResponse <Success: True; Response Elements: 2>

    In [46]: for r in reply:
        ...:     print(r)
        ...:     print(r.result)
        ...:
    Response <Success: True>
    *08:38:20.115 UTC Thu Apr 1 2021
    Response <Success: True>
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    Ethernet0/2                unassigned      YES NVRAM  up                    up
    Ethernet0/3                192.168.130.1   YES NVRAM  up                    up

    In [47]: reply.result
    Out[47]: 'sh clock\n*08:38:20.115 UTC Thu Apr 1 2021sh ip int br\nInterface                  IP-Address      OK? Method Status                Protocol\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up\nEthernet0/2                unassigned      YES NVRAM  up                    up\nEthernet0/3                192.168.130.1   YES NVRAM  up                    up'

    In [48]: reply[0]
    Out[48]: Response <Success: True>

    In [49]: reply[1]
    Out[49]: Response <Success: True>

    In [50]: reply[0].result
    Out[50]: '*08:38:20.115 UTC Thu Apr 1 2021'

When sending multiple commands, it is also very convenient to use
the ``stop_on_failed`` parameter. By default, it is False, so all commands
are executed, but if you specify ``stop_on_failed=True``, after an error
occurs in some command, the following commands will not be executed:

.. code:: python

    In [59]: reply = ssh.send_commands(["ping 192.168.100.2", "sh clck", "sh ip int br"], stop_on_failed=True)

    In [60]: reply
    Out[60]: MultiResponse <Success: False; Response Elements: 2>

    In [61]: reply.result
    Out[61]: "ping 192.168.100.2\nType escape sequence to abort.\nSending 5, 100-byte ICMP Echos to 192.168.100.2, timeout is 2 seconds:\n!!!!!\nSuccess rate is 100 percent (5/5), round-trip min/avg/max = 1/2/6 mssh clck\n        ^\n% Invalid input detected at '^' marker."

    In [62]: for r in reply:
        ...:     print(r)
        ...:     print(r.result)
        ...:
    Response <Success: True>
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.100.2, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/6 ms
    Response <Success: False>
            ^
    % Invalid input detected at '^' marker.


Telnet connection
~~~~~~~~~~~~~~~~~~

To connect to equipment via Telnet, you must specify transport equal to ``telnet``
and be sure to specify the port parameter equal to 23 (or the port that you use
to connect via Telnet):

.. code:: python

    from scrapli.driver.core import IOSXEDriver
    from scrapli.exceptions import ScrapliException
    import socket

    r1 = {
        "host": "192.168.100.1",
        "auth_username": "cisco",
        "auth_password": "cisco2",
        "auth_secondary": "cisco",
        "auth_strict_key": False,
        "transport": "telnet",
        "port": 23,  # must be specified when connecting telnet
    }


    def send_show(device, show_command):
        try:
            with IOSXEDriver(**r1) as ssh:
                reply = ssh.send_command(show_command)
                return reply.result
        except socket.timeout as error:
            print(error)
        except ScrapliException as error:
            print(error, device["host"])


    if __name__ == "__main__":
        output = send_show(r1, "sh ip int br")
        print(output)


Scrapli examples
~~~~~~~~~~~~~~~~

Basic example of sending show command:

.. code:: python

    from scrapli.driver.core import IOSXEDriver
    from scrapli.exceptions import ScrapliException


    r1 = {
        "host": "192.168.100.1",
        "auth_username": "cisco",
        "auth_password": "cisco",
        "auth_secondary": "cisco",
        "auth_strict_key": False,
        "timeout_socket": 5,  # timeout for establishing socket/initial connection
        "timeout_transport": 10,  # timeout for ssh|telnet transport
    }


    def send_show(device, show_command):
        try:
            with IOSXEDriver(**r1) as ssh:
                reply = ssh.send_command(show_command)
                return reply.result
        except ScrapliException as error:
            print(error, device["host"])


    if __name__ == "__main__":
        output = send_show(r1, "sh ip int br")
        print(output)

Basic example of sending config commands:

.. code:: python

    from pprint import pprint
    from scrapli import Scrapli

    r1 = {
        "host": "192.168.100.1",
        "auth_username": "cisco",
        "auth_password": "cisco",
        "auth_secondary": "cisco",
        "auth_strict_key": False,
        "platform": "cisco_iosxe",
    }


    def send_show(device, show_commands):
        if type(show_commands) == str:
            show_commands = [show_commands]
        cmd_dict = {}
        with Scrapli(**r1) as ssh:
            for cmd in show_commands:
                reply = ssh.send_command(cmd)
                cmd_dict[cmd] = reply.result
        return cmd_dict


    if __name__ == "__main__":
        print("show".center(20, "#"))
        output = send_show(r1, ["sh ip int br", "sh ver | i uptime"])
        pprint(output, width=120)

An example of sending configuration commands with error checking:

.. code:: python

    from pprint import pprint
    from scrapli import Scrapli

    r1 = {
        "host": "192.168.100.1",
        "auth_username": "cisco",
        "auth_password": "cisco",
        "auth_secondary": "cisco",
        "auth_strict_key": False,
        "platform": "cisco_iosxe",
    }


    def send_cfg(device, cfg_commands, strict=False):
        output = ""
        if type(cfg_commands) == str:
            cfg_commands = [cfg_commands]
        with Scrapli(**r1) as ssh:
            reply = ssh.send_configs(cfg_commands, stop_on_failed=strict)
            for cmd_reply in reply:
                if cmd_reply.failed:
                    print(
                        f"An error occurred while executing the command::\n{reply.result}\n"
                    )
            output = reply.result
        return output


    if __name__ == "__main__":
        output_cfg = send_cfg(
            r1, ["interfacelo11", "ip address 11.1.1.1 255.255.255.255"], strict=True
        )
        print(output_cfg)

