Module paramiko
---------------

Paramiko is an implementation of SSHv2 protocol on Python. Paramiko provides client-server functionality. We will consider only client functionality.

Since Paramiko is not part of standard Python module library, it needs to be installed:

::

    pip install paramiko

Connection is established in this way: first, client is created and client configuration is set, then connection is initiated and an interactive session is received:

.. code:: python

    In [2]: client = paramiko.SSHClient()

    In [3]: client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

    In [4]: client.connect(hostname="192.168.100.1", username="cisco", password="cisco",
       ...: look_for_keys=False, allow_agent=False)

    In [5]: ssh = client.invoke_shell()

SSHClient is a class that represents a connection to SSH server. It performs client authentication.
String ``set_missing_host_key_policy`` is optional, it indicates
which policy to use when connecting to a server whose key is unknown.
Policy ``paramiko.AutoAddPolicy()`` automatically add new hostname and key to local HostKeys object.

Method ``connect`` connects to SSH server and authenticates the connection. Parameters:

* ``look_for_keys`` - by default paramiko performs key authentication. To disable this, put the flag in False
* ``allow_agent`` - paramiko can connect to a local SSH agent. This is necessary when working with keys and since in this case authentication is done by login/password, it should be disabled.  

After execution of previous command there is already a connection to server. Method ``invoke_shell`` allows to set an interactive SSH session with server.

Method send
~~~~~~~~~~

Method ``send`` - sends specified string to session and returns amount of sent bytes.

.. code:: python

    In [7]: ssh.send("enable\n")
    Out[7]: 7

    In [8]: ssh.send("cisco\n")
    Out[8]: 6

    In [9]: ssh.send("sh ip int br\n")
    Out[9]: 13


.. warning::

     In code, after send() you will need to put time.sleep, especially between send and recv. Since this is an interactive session and commands are slow to type, everything works without pauses.

Method recv
~~~~~~~~~~

Method ``recv`` receives data from session. In brackets, the maximum value in bytes that can be obtained is indicated. This method returns a received string

.. code:: python

    In [10]: ssh.recv(3000)
    Out[10]: b'\r\nR1>enable\r\nPassword: \r\nR1#sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                unassigned      YES NVRAM  up                    up      \r\nEthernet0/3                192.168.130.1   YES NVRAM  up                    up      \r\nLoopback22                 10.2.2.2        YES manual up                    up      \r\nLoopback33                 unassigned      YES unset  up                    up      \r\nLoopback45                 unassigned      YES unset  up                    up      \r\nLoopback55                 5.5.5.5         YES manual up                    up      \r\nR1#'

Method close
~~~~~~~~~~~

Method close() closes session:

.. code:: python

    In [11]: ssh.close()


Example of paramiko use
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Example of paramiko use (3_paramiko.py file):

.. code:: python

    import paramiko
    import time
    import socket
    from pprint import pprint


    def send_show_command(
        ip,
        username,
        password,
        enable,
        command,
        max_bytes=60000,
        short_pause=1,
        long_pause=5,
    ):
        cl = paramiko.SSHClient()
        cl.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        cl.connect(
            hostname=ip,
            username=username,
            password=password,
            look_for_keys=False,
            allow_agent=False,
        )
        with cl.invoke_shell() as ssh:
            ssh.send("enable\n")
            ssh.send(f"{enable}\n")
            time.sleep(short_pause)
            ssh.send("terminal length 0\n")
            time.sleep(short_pause)
            ssh.recv(max_bytes)

            result = {}
            for command in commands:
                ssh.send(f"{command}\n")
                ssh.settimeout(5)

                output = ""
                while True:
                    try:
                        part = ssh.recv(max_bytes).decode("utf-8")
                        output += part
                        time.sleep(0.5)
                    except socket.timeout:
                        break
                result[command] = output

            return result


    if __name__ == "__main__":
        devices = ["192.168.100.1", "192.168.100.2", "192.168.100.3"]
        commands = ["sh clock", "sh arp"]
        result = send_show_command("192.168.100.1", "cisco", "cisco", "cisco", commands)
        pprint(result, width=120)



Result of script execution:

::

    {'sh arp': 'sh arp\r\n'
               'Protocol  Address          Age (min)  Hardware Addr   Type   Interface\r\n'
               'Internet  192.168.100.1           -   aabb.cc00.6500  ARPA   Ethernet0/0\r\n'
               'Internet  192.168.100.2         124   aabb.cc00.6600  ARPA   Ethernet0/0\r\n'
               'Internet  192.168.100.3         183   aabb.cc00.6700  ARPA   Ethernet0/0\r\n'
               'Internet  192.168.100.100       208   aabb.cc80.c900  ARPA   Ethernet0/0\r\n'
               'Internet  192.168.101.1           -   aabb.cc00.6500  ARPA   Ethernet0/0\r\n'
               'Internet  192.168.102.1           -   aabb.cc00.6500  ARPA   Ethernet0/0\r\n'
               'Internet  192.168.130.1           -   aabb.cc00.6530  ARPA   Ethernet0/3\r\n'
               'Internet  192.168.200.1           -   0203.e800.6510  ARPA   Ethernet0/1\r\n'
               'Internet  192.168.200.100        18   6ee2.6d8c.e75d  ARPA   Ethernet0/1\r\n'
               'R1#',
     'sh clock': 'sh clock\r\n*08:25:22.435 UTC Mon Jul 20 2020\r\nR1#'}


Paginated command output
~~~~~~~~~~~~~~~~~~~~~~~~~

Example of using paramiko to work with paginated output of *show* command (3_paramiko_more.py file):

.. code:: python

    import paramiko
    import time
    import socket
    from pprint import pprint
    import re


    def send_show_command(
        ip,
        username,
        password,
        enable,
        command,
        max_bytes=60000,
        short_pause=1,
        long_pause=5,
    ):
        cl = paramiko.SSHClient()
        cl.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        cl.connect(
            hostname=ip,
            username=username,
            password=password,
            look_for_keys=False,
            allow_agent=False,
        )
        with cl.invoke_shell() as ssh:
            ssh.send("enable\n")
            ssh.send(enable + "\n")
            time.sleep(short_pause)
            ssh.recv(max_bytes)

            result = {}
            for command in commands:
                ssh.send(f"{command}\n")
                ssh.settimeout(5)

                output = ""
                while True:
                    try:
                        page = ssh.recv(max_bytes).decode("utf-8")
                        output += page
                        time.sleep(0.5)
                    except socket.timeout:
                        break
                    if "More" in page:
                        ssh.send(" ")
                output = re.sub(" +--More--| +\x08+ +\x08+", "\n", output)
                result[command] = output

            return result


    if __name__ == "__main__":
        devices = ["192.168.100.1", "192.168.100.2", "192.168.100.3"]
        commands = ["sh run"]
        result = send_show_command("192.168.100.1", "cisco", "cisco", "cisco", commands)
        pprint(result, width=120)

