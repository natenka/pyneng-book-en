Module telnetlib
----------------

Module telnetlib is part of standard Python library. This is telnet client implementation.

.. note::

    It is also possible to connect via telnet using pexpect. Plus of telnetlib is that this module is part of standard Python library.
    
Telnetlib resembles pexpect but has several differences. The most notable difference is that telnetlib requires a transfer of a byte string, rather than normal one.

Connection is performed as follows:

.. code:: python

    In [1]: telnet = telnetlib.Telnet('192.168.100.1')

Method read_until
~~~~~~~~~~~~~~~~

Method read_until() specifies till which line the output should be read. However, as an argument, it is necessary to pass bytes, not the usual string:

.. code:: python

    In [2]: telnet.read_until(b'Username')
    Out[2]: b'\r\n\r\nUser Access Verification\r\n\r\nUsername'

Method read_until() returns everything it has read before specified string.

Method write
~~~~~~~~~~~

Method write() is used for data transmission. Byte string has to be passed to it:

.. code:: python

    In [3]: telnet.write(b'cisco\n')

Read output till *Password* and pass the password:

.. code:: python

    In [4]: telnet.read_until(b'Password')
    Out[4]: b': cisco\r\nPassword'

    In [5]: telnet.write(b'cisco\n')

You can now specify what should be read untill prompt and then send the command:

.. code:: python

    In [6]: telnet.read_until(b'>')
    Out[6]: b': \r\nR1>'

    In [7]: telnet.write(b'sh ip int br\n')

After sending a command, you can continue to use read_until() method:

.. code:: python

    In [8]: telnet.read_until(b'>')
    Out[8]: b'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nR1>'

Method read_very_eager
~~~~~~~~~~~~~~~~~~~~~

Or use another read method read_very_eager(). When using read_very_eager() method, you can send multiple commands and then read all available output:

.. code:: python

    In [9]: telnet.write(b'sh arp\n')

    In [10]: telnet.write(b'sh clock\n')

    In [11]: telnet.write(b'sh ip int br\n')

    In [12]: all_result = telnet.read_very_eager().decode('utf-8')

    In [13]: print(all_result)
    sh arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  10.30.0.1               -   aabb.cc00.6530  ARPA   Ethernet0/3.300
    Internet  10.100.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.100
    Internet  10.200.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.200
    Internet  19.1.1.1                -   aabb.cc00.6520  ARPA   Ethernet0/2
    Internet  192.168.100.1           -   aabb.cc00.6500  ARPA   Ethernet0/0
    Internet  192.168.100.2         124   aabb.cc00.6600  ARPA   Ethernet0/0
    Internet  192.168.100.3         143   aabb.cc00.6700  ARPA   Ethernet0/0
    Internet  192.168.100.100       160   aabb.cc80.c900  ARPA   Ethernet0/0
    Internet  192.168.200.1           -   0203.e800.6510  ARPA   Ethernet0/1
    Internet  192.168.200.100        13   0800.27ac.16db  ARPA   Ethernet0/1
    Internet  192.168.230.1           -   aabb.cc00.6530  ARPA   Ethernet0/3
    R1>sh clock
    *19:18:57.980 UTC Fri Nov 3 2017
    R1>sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    Ethernet0/2                19.1.1.1        YES NVRAM  up                    up
    Ethernet0/3                192.168.230.1   YES NVRAM  up                    up
    Ethernet0/3.100            10.100.0.1      YES NVRAM  up                    up
    Ethernet0/3.200            10.200.0.1      YES NVRAM  up                    up
    Ethernet0/3.300            10.30.0.1       YES NVRAM  up                    up
    R1>

.. warning::

    You should always set time.sleep(n) before using read_very_eager.

With read_until() will be a slightly different approach. You can execute the same three commands, but then get the output one by one because of reading till prompt string:

.. code:: python

    In [14]: telnet.write(b'sh arp\n')

    In [15]: telnet.write(b'sh clock\n')

    In [16]: telnet.write(b'sh ip int br\n')

    In [17]: telnet.read_until(b'>')
    Out[17]: b'sh arp\r\nProtocol  Address          Age (min)  Hardware Addr   Type   Interface\r\nInternet  10.30.0.1               -   aabb.cc00.6530  ARPA   Ethernet0/3.300\r\nInternet  10.100.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.100\r\nInternet  10.200.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.200\r\nInternet  19.1.1.1                -   aabb.cc00.6520  ARPA   Ethernet0/2\r\nInternet  192.168.100.1           -   aabb.cc00.6500  ARPA   Ethernet0/0\r\nInternet  192.168.100.2         126   aabb.cc00.6600  ARPA   Ethernet0/0\r\nInternet  192.168.100.3         145   aabb.cc00.6700  ARPA   Ethernet0/0\r\nInternet  192.168.100.100       162   aabb.cc80.c900  ARPA   Ethernet0/0\r\nInternet  192.168.200.1           -   0203.e800.6510  ARPA   Ethernet0/1\r\nInternet  192.168.200.100        15   0800.27ac.16db  ARPA   Ethernet0/1\r\nInternet  192.168.230.1           -   aabb.cc00.6530  ARPA   Ethernet0/3\r\nR1>'

    In [18]: telnet.read_until(b'>')
    Out[18]: b'sh clock\r\n*19:20:39.388 UTC Fri Nov 3 2017\r\nR1>'

    In [19]: telnet.read_until(b'>')
    Out[19]: b'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nR1>'

read_until vs read_very_eager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An important difference between read_until() and read_very_eager() is how they react to the lack of output.

Method read_until() waits for a certain string. By default, if it does not exist, method will "freeze". Timeout option allows you to specify how long to wait for the desired string:

.. code:: python

    In [20]: telnet.read_until(b'>', timeout=5)
    Out[20]: b''

If no string appears during the specified time, an empty string is returned.

Method read_very_eager() simply returns an empty string if there is no output:

.. code:: python

    In [21]: telnet.read_very_eager()
    Out[21]: b''


Method expect
~~~~~~~~~~~~

Method expect() allows you to specify a list with regular expressions. It works like pexpect but telnetlib always has to pass a list of regular expressions.

You can then transfer byte strings or compiled regular expressions:

.. code:: python

    In [22]: telnet.write(b'sh clock\n')

    In [23]: telnet.expect([b'[>#]'])
    Out[23]:
    (0,
     <_sre.SRE_Match object; span=(46, 47), match=b'>'>,
     b'sh clock\r\n*19:35:10.984 UTC Fri Nov 3 2017\r\nR1>')

Method expect() returns tuple of their three elements:

* index of matched expression 
* object Match 
* byte string that contains everything read till regular expression including regular expression

Accordingly, if necessary you can continue working with these elements:

.. code:: python

    In [24]: telnet.write(b'sh clock\n')

    In [25]: regex_idx, match, output = telnet.expect([b'[>#]'])

    In [26]: regex_idx
    Out[26]: 0

    In [27]: match.group()
    Out[27]: b'>'

    In [28]: match
    Out[28]: <_sre.SRE_Match object; span=(46, 47), match=b'>'>

    In [29]: match.group()
    Out[29]: b'>'

    In [30]: output
    Out[30]: b'sh clock\r\n*19:37:21.577 UTC Fri Nov 3 2017\r\nR1>'

    In [31]: output.decode('utf-8')
    Out[31]: 'sh clock\r\n*19:37:21.577 UTC Fri Nov 3 2017\r\nR1>'

Method close
~~~~~~~~~~~

Method close() closes connection but it's better to open and close connection using context manager:

.. code:: python

    In [32]: telnet.close()

.. note::

    Using Telnet object as context manager added in version 3.6

Telnetlib usage example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Working principle of telnetlib resembles pexpect, so the example below should be clear.

File 2_telnetlib.py:

.. code:: python

    import telnetlib
    import time
    from pprint import pprint


    def to_bytes(line):
        return f"{line}\n".encode("utf-8")


    def send_show_command(ip, username, password, enable, commands):
        with telnetlib.Telnet(ip) as telnet:
            telnet.read_until(b"Username")
            telnet.write(to_bytes(username))
            telnet.read_until(b"Password")
            telnet.write(to_bytes(password))
            index, m, output = telnet.expect([b">", b"#"])
            if index == 0:
                telnet.write(b"enable\n")
                telnet.read_until(b"Password")
                telnet.write(to_bytes(enable))
                telnet.read_until(b"#", timeout=5)
            telnet.write(b"terminal length 0\n")
            telnet.read_until(b"#", timeout=5)
            time.sleep(3)
            telnet.read_very_eager()

            result = {}
            for command in commands:
                telnet.write(to_bytes(command))
                output = telnet.read_until(b"#", timeout=5).decode("utf-8")
                result[command] = output.replace("\r\n", "\n")
            return result


    if __name__ == "__main__":
        devices = ["192.168.100.1", "192.168.100.2", "192.168.100.3"]
        commands = ["sh ip int br", "sh arp"]
        for ip in devices:
            result = send_show_command(ip, "cisco", "cisco", "cisco", commands)
            pprint(result, width=120)

Since bytes need to be passed to ``write`` method and new line character should
be added each time, a small function ``to_bytes`` is created that does the
conversion to bytes and adds a new line.

Script execution:

::

    {'sh int desc': 'sh int desc\n'
                    'Interface             Status         Protocol Description\n'
                    'Et0/0                 up             up       \n'
                    'Et0/1                 up             up       \n'
                    'Et0/2                 up             up       \n'
                    'Et0/3                 up             up       \n'
                    'R1#',
     'sh ip int br': 'sh ip int br\n'
                     'Interface         IP-Address      OK? Method Status                Protocol\n'
                     'Ethernet0/0       192.168.100.1   YES NVRAM  up                    up      \n'
                     'Ethernet0/1       192.168.200.1   YES NVRAM  up                    up      \n'
                     'Ethernet0/2       unassigned      YES NVRAM  up                    up      \n'
                     'Ethernet0/3       192.168.130.1   YES NVRAM  up                    up      \n'
                     'R1#'}
    {'sh int desc': 'sh int desc\n'
                    'Interface             Status         Protocol Description\n'
                    'Et0/0                 up             up       \n'
                    'Et0/1                 up             up       \n'
                    'Et0/2                 admin down     down     \n'
                    'Et0/3                 admin down     down     \n'
                    'R2#',
     'sh ip int br': 'sh ip int br\n'
                     'Interface         IP-Address      OK? Method Status                Protocol\n'
                     'Ethernet0/0       192.168.100.2   YES NVRAM  up                    up      \n'
                     'Ethernet0/1       unassigned      YES NVRAM  up                    up      \n'
                     'Ethernet0/2       unassigned      YES NVRAM  administratively down down    \n'
                     'Ethernet0/3       unassigned      YES NVRAM  administratively down down    \n'
                     'R2#'}
    {'sh int desc': 'sh int desc\n'
                    'Interface             Status         Protocol Description\n'
                    'Et0/0                 up             up       \n'
                    'Et0/1                 up             up       \n'
                    'Et0/2                 admin down     down     \n'
                    'Et0/3                 admin down     down     \n'
                    'R3#',
     'sh ip int br': 'sh ip int br\n'
                     'Interface         IP-Address      OK? Method Status                Protocol\n'
                     'Ethernet0/0       192.168.100.3   YES NVRAM  up                    up      \n'
                     'Ethernet0/1       unassigned      YES NVRAM  up                    up      \n'
                     'Ethernet0/2       unassigned      YES NVRAM  administratively down down    \n'
                     'Ethernet0/3       unassigned      YES NVRAM  administratively down down    \n'
                     
Paginated command output
~~~~~~~~~~~~~~~~~~~~~~~~~

Example of using telnetlib to work with paginated output of *show* commands (2_telnetlib_more.py file):

.. code:: python

    import telnetlib
    import time
    from pprint import pprint
    import re


    def to_bytes(line):
        return f"{line}\n".encode("utf-8")


    def send_show_command(ip, username, password, enable, command):
        with telnetlib.Telnet(ip) as telnet:
            telnet.read_until(b"Username")
            telnet.write(to_bytes(username))
            telnet.read_until(b"Password")
            telnet.write(to_bytes(password))
            index, m, output = telnet.expect([b">", b"#"])
            if index == 0:
                telnet.write(b"enable\n")
                telnet.read_until(b"Password")
                telnet.write(to_bytes(enable))
                telnet.read_until(b"#", timeout=5)
            time.sleep(3)
            telnet.read_very_eager()

            telnet.write(to_bytes(command))
            result = ""

            while True:
                index, match, output = telnet.expect([b"--More--", b"#"], timeout=5)
                output = output.decode("utf-8")
                output = re.sub(" +--More--| +\x08+ +\x08+", "\n", output)
                result += output
                if index in (1, -1):
                    break
                telnet.write(b" ")
                time.sleep(1)
                result.replace("\r\n", "\n")

            return result


    if __name__ == "__main__":
        devices = ["192.168.100.1", "192.168.100.2", "192.168.100.3"]
        for ip in devices:
            result = send_show_command(ip, "cisco", "cisco", "cisco", "sh run")
            pprint(result, width=120)
