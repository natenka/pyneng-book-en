Examples of converting between bytes and strings
--------------------------------------------

Consider a few examples of working with bytes and converting bytes to string.

subprocess
~~~~~~~~~~

Module subprocess returns the result of command as bytes:

.. code:: python

    In [1]: import subprocess

    In [2]: result = subprocess.run(['ping', '-c', '3', '-n', '8.8.8.8'],
       ...:                         stdout=subprocess.PIPE)
       ...:

    In [3]: result.stdout
    Out[3]: b'PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.\n64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=59.4 ms\n64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=54.4 ms\n64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=55.1 ms\n\n--- 8.8.8.8 ping statistics ---\n3 packets transmitted, 3 received, 0% packet loss, time 2002ms\nrtt min/avg/max/mdev = 54.470/56.346/59.440/2.220 ms\n'

If it is necessary to work with this output further you should immediately convert it to string:

.. code:: python

    In [4]: output = result.stdout.decode('utf-8')

    In [5]: print(output)
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=59.4 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=54.4 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=55.1 ms

    --- 8.8.8.8 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2002ms
    rtt min/avg/max/mdev = 54.470/56.346/59.440/2.220 ms

Module subprocess supports another conversion option - ``encoding`` parameter.
If you specify it when you call run() function, the result will be as a string:

.. code:: python

    In [6]: result = subprocess.run(['ping', '-c', '3', '-n', '8.8.8.8'],
       ...:                         stdout=subprocess.PIPE, encoding='utf-8')
       ...:

    In [7]: result.stdout
    Out[7]: 'PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.\n64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=55.5 ms\n64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=54.6 ms\n64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=53.3 ms\n\n--- 8.8.8.8 ping statistics ---\n3 packets transmitted, 3 received, 0% packet loss, time 2003ms\nrtt min/avg/max/mdev = 53.368/54.534/55.564/0.941 ms\n'

    In [8]: print(result.stdout)
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=55.5 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=54.6 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=53.3 ms

    --- 8.8.8.8 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2003ms
    rtt min/avg/max/mdev = 53.368/54.534/55.564/0.941 ms

telnetlib
~~~~~~~~~

Depending on module, conversion between strings and bytes can be performed
automatically or may be required explicitly.

For example, ``telnetlib`` module must pass bytes to ``read_until`` and
``write`` methods:

.. code:: python

    import telnetlib
    import time

    t = telnetlib.Telnet('192.168.100.1')

    t.read_until(b'Username:')
    t.write(b'cisco\n')

    t.read_until(b'Password:')
    t.write(b'cisco\n')
    t.write(b'sh ip int br\n')

    time.sleep(5)

    output = t.read_very_eager().decode('utf-8')
    print(output)

Method returns bytes, so penultimate line uses decode.

pexpect
~~~~~~~

Module ``pexpect`` waits for a string as an argument and returns bytes:

.. code:: python

    In [9]: import pexpect

    In [10]: output = pexpect.run('ls -ls')

    In [11]: output
    Out[11]: b'total 8\r\n4 drwxr-xr-x 2 vagrant vagrant 4096 Aug 28 12:16 concurrent_futures\r\n4 drwxr-xr-x 2 vagrant vagrant 4096 Aug  3 07:59 iterator_generator\r\n'

    In [12]: output.decode('utf-8')
    Out[12]: 'total 8\r\n4 drwxr-xr-x 2 vagrant vagrant 4096 Aug 28 12:16 concurrent_futures\r\n4 drwxr-xr-x 2 vagrant vagrant 4096 Aug  3 07:59 iterator_generator\r\n'

And it also supports ``encoding`` parameter:

.. code:: python

    In [13]: output = pexpect.run('ls -ls', encoding='utf-8')

    In [14]: output
    Out[14]: 'total 8\r\n4 drwxr-xr-x 2 vagrant vagrant 4096 Aug 28 12:16 concurrent_futures\r\n4 drwxr-xr-x 2 vagrant vagrant 4096 Aug  3 07:59 iterator_generator\r\n'

Working with files
~~~~~~~~~~~~~~~~~~

Until now, when working with files, the following expression was used:

.. code:: python

    with open(filename) as f:
        for line in f:
            print(line)

But actually, when you read a file you convert bytes to a string. And default
encoding was used:

.. code:: python

    In [1]: import locale

    In [2]: locale.getpreferredencoding()
    Out[2]: 'UTF-8'

Default encoding in file:

.. code:: python

    In [2]: f = open('r1.txt')

    In [3]: f
    Out[3]: <_io.TextIOWrapper name='r1.txt' mode='r' encoding='UTF-8'>

When working with files it is better to specify encoding explicitly because
it may differ in different operating systems:

.. code:: python

    In [4]: with open('r1.txt', encoding='utf-8') as f:
       ...:     for line in f:
       ...:         print(line, end='')
       ...:
    !
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    !
    no ip domain lookup
    !
    ip ssh version 2
    !

Conclusion
~~~~~~~~~~

These examples are shown here to show that different modules can treat the issue
of conversion between strings and bytes differently. And different functions and
methods of these modules can expect arguments and return values of different
types. However, all of these items are in documentation.
