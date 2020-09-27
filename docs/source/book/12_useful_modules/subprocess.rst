Subprocess
-----------------

Subprocess module allows you to create new processes. It can then connect to `standard input/output/error streams <http://xgu.ru/wiki/stdin>`__ and receive a return code.

Subprocess can for example execute any Linux commands from the script. And depending on the situation get the output or just check that command has been performed correctly.

.. note::
    In Python 3.5, syntax of subprocess module has changed.

Function ``subprocess.run()``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Function ``subprocess.run()`` is the main way of working with the subprocess module.

The easiest way to use a function is to call it in this way:

.. code:: python

    In [1]: import subprocess

    In [2]: result = subprocess.run('ls')
    ipython_as_mngmt_console.md  README.md         version_control.md
    module_search.md             useful_functions
    naming_conventions           useful_modules

The **result** variable now contains a special CompletedProcess object. From this object you can get information about the execution of the process, such as the return code:

.. code:: python

    In [3]: result
    Out[3]: CompletedProcess(args='ls', returncode=0)

    In [4]: result.returncode
    Out[4]: 0

Code 0 means that program was executed successfully.

If it is necessary to call a command with arguments, it should be passed in this way (as a list):

.. code:: python

    In [5]: result = subprocess.run(['ls', '-ls'])
    total 28
    4 -rw-r--r-- 1 vagrant vagrant   56 Jun  7 19:35 ipython_as_mngmt_console.md
    4 -rw-r--r-- 1 vagrant vagrant 1638 Jun  7 19:35 module_search.md
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun  7 19:35 naming_conventions
    4 -rw-r--r-- 1 vagrant vagrant  277 Jun  7 19:35 README.md
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 16 05:11 useful_functions
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 17 16:28 useful_modules
    4 -rw-r--r-- 1 vagrant vagrant   49 Jun  7 19:35 version_control.md

Trying to execute a command using wildcard expressions, for example using ``*``, will cause an error:

.. code:: python

    In [6]: result = subprocess.run(['ls', '-ls', '*md'])
    ls: cannot access *md: No such file or directory

To call commands in which wildcard expressions are used, you add a **shell** argument and call the command:

.. code:: python

    In [7]: result = subprocess.run('ls -ls *md', shell=True)
    4 -rw-r--r-- 1 vagrant vagrant   56 Jun  7 19:35 ipython_as_mngmt_console.md
    4 -rw-r--r-- 1 vagrant vagrant 1638 Jun  7 19:35 module_search.md
    4 -rw-r--r-- 1 vagrant vagrant  277 Jun  7 19:35 README.md
    4 -rw-r--r-- 1 vagrant vagrant   49 Jun  7 19:35 version_control.md

Another feature of the ``run()`` If you try to run a ping command, for example, this aspect will be visible:

.. code:: python

    In [8]: result = subprocess.run(['ping', '-c', '3', '-n', '8.8.8.8'])
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=55.1 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=54.7 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=54.4 ms

    --- 8.8.8.8 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2004ms
    rtt min/avg/max/mdev = 54.498/54.798/55.116/0.252 ms

Getting the result of a command execution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, the run() function returns the result of a command execution to a standard output stream. If you want to get the result of command execution, add **stdout** argument with  value **subprocess.PIPE**:

.. code:: python

    In [9]: result = subprocess.run(['ls', '-ls'], stdout=subprocess.PIPE)

Now you can get the result of command executing in this way:

.. code:: python

    In [10]: print(result.stdout)
    b'total 28\n4 -rw-r--r-- 1 vagrant vagrant   56 Jun  7 19:35 ipython_as_mngmt_console.md\n4 -rw-r--r-- 1 vagrant vagrant 1638 Jun  7 19:35 module_search.md\n4 drwxr-xr-x 2 vagrant vagrant 4096 Jun  7 19:35 naming_conventions\n4 -rw-r--r-- 1 vagrant vagrant  277 Jun  7 19:35 README.md\n4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 16 05:11 useful_functions\n4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 17 16:30 useful_modules\n4 -rw-r--r-- 1 vagrant vagrant   49 Jun  7 19:35 version_control.md\n'

Note letter **b** before line. It means that module returned the output as a byte string. There are two options to translate it into unicode:

-  decode received string
-  specify encoding argument

Example with decode:

.. code:: python

    In [11]: print(result.stdout.decode('utf-8'))
    total 28
    4 -rw-r--r-- 1 vagrant vagrant   56 Jun  7 19:35 ipython_as_mngmt_console.md
    4 -rw-r--r-- 1 vagrant vagrant 1638 Jun  7 19:35 module_search.md
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun  7 19:35 naming_conventions
    4 -rw-r--r-- 1 vagrant vagrant  277 Jun  7 19:35 README.md
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 16 05:11 useful_functions
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 17 16:30 useful_modules
    4 -rw-r--r-- 1 vagrant vagrant   49 Jun  7 19:35 version_control.md

Example with encoding:

.. code:: python

    In [12]: result = subprocess.run(['ls', '-ls'], stdout=subprocess.PIPE, encoding='utf-8')

    In [13]: print(result.stdout)
    total 28
    4 -rw-r--r-- 1 vagrant vagrant   56 Jun  7 19:35 ipython_as_mngmt_console.md
    4 -rw-r--r-- 1 vagrant vagrant 1638 Jun  7 19:35 module_search.md
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun  7 19:35 naming_conventions
    4 -rw-r--r-- 1 vagrant vagrant  277 Jun  7 19:35 README.md
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 16 05:11 useful_functions
    4 drwxr-xr-x 2 vagrant vagrant 4096 Jun 17 16:31 useful_modules
    4 -rw-r--r-- 1 vagrant vagrant   49 Jun  7 19:35 version_control.md

Output disabling
~~~~~~~~~~~~~~~~~

Sometimes it is enough to get only return code and need to disable output of execution result on standard output stream. This can be done by passing to run() function the **stdout**  argument with value **subprocess.DEVNULL**:

.. code:: python

    In [14]: result = subprocess.run(['ls', '-ls'], stdout=subprocess.DEVNULL)

    In [15]: print(result.stdout)
    None

    In [16]: print(result.returncode)
    0

Working with standard error stream
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the command was executed with error or failed, the output of command will fall on standard error stream.

This can be obtained in the same way as the standard output stream:

.. code:: python

    In [17]: result = subprocess.run(['ping', '-c', '3', '-n', 'a'], stderr=subprocess.PIPE, encoding='utf-8')

Now result.stdout has empty string and result.stderr has standard output stream:

.. code:: python

    In [18]: print(result.stdout)
    None

    In [19]: print(result.stderr)
    ping: unknown host a


    In [20]: print(result.returncode)
    2

Примеры использования модуля
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example of subprocess module use (subprocess_run_basic.py file):

.. code:: python

    import subprocess

    reply = subprocess.run(['ping', '-c', '3', '-n', '8.8.8.8'])

    if reply.returncode == 0:
        print('Alive')
    else:
        print('Unreachable')

The result will be:

.. code:: python

    $ python subprocess_run_basic.py
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=54.0 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=54.4 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=53.9 ms

    --- 8.8.8.8 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2005ms
    rtt min/avg/max/mdev = 53.962/54.145/54.461/0.293 ms
    Alive

That is, the result of command execution is printed to standard output stream.

The ping_ip function checks the availability of the IP address and returns True and **stdout** if address is available, or False and **stderr** if address is not available (subprocess\_ping\_function.py file):

.. code:: python

    import subprocess


    def ping_ip(ip_address):
        """
        Ping IP address and return tuple:
        On success:
            * True
            * command output (stdout)
        On failure:
            * False
            * error output (stderr)
        """
        reply = subprocess.run(['ping', '-c', '3', '-n', ip_address],
                               stdout=subprocess.PIPE,
                               stderr=subprocess.PIPE,
                               encoding='utf-8')
        if reply.returncode == 0:
            return True, reply.stdout
        else:
            return False, reply.stderr

    print(ping_ip('8.8.8.8'))
    print(ping_ip('a'))

The result will be:

::

    $ python subprocess_ping_function.py
    (True, 'PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.\n64 bytes from 8.8.8.8: icmp_seq=1 ttl=43 time=63.8 ms\n64 bytes from 8.8.8.8: icmp_seq=2 ttl=43 time=55.6 ms\n64 bytes from 8.8.8.8: icmp_seq=3 ttl=43 time=55.9 ms\n\n--- 8.8.8.8 ping statistics ---\n3 packets transmitted, 3 received, 0% packet loss, time 2003ms\nrtt min/avg/max/mdev = 55.643/58.492/63.852/3.802 ms\n')
    (False, 'ping: unknown host a\n')

Based on this function you can make a function that will check the list of IP addresses and return as a result two lists: accessible and inaccessible addresses.

.. note::
    You will find it in tasks of section

If the number of IP addresses to check is large, you can use threading or multiprocessing modules to speed up verification.
