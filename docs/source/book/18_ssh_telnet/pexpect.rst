Module pexpect
--------------

Module pexpect allows to automate interactive connections such as:

* telnet 
* ssh 
* ftp

.. note::

    Pexpect is an implementation of *expect* in Python.

First, pexpect module needs to be installed:

::

    pip install pexpect


The logic of pexpect is:

* some program is running
* pexpect expects a certain output (prompt, password request, etc.) 
* after receiving the output, it sends commands/data
* last two actions are repeated as many times as necessary

At the same time, pexpect does not implement utilities but uses ready-made ones.

``pexpect.spawn``
~~~~~~~~~~~~~~~~~

Class ``spawn`` allows you to interact with called program by sending data and
waiting for a response.

For example, you can initiate SSH connecton:

.. code:: python

    In [5]: ssh = pexpect.spawn('ssh cisco@192.168.100.1')

After executing this line, connection is established. Now you must specify
which line to expect. In this case, wait for password request:

.. code:: python

    In [6]: ssh.expect('[Pp]assword')
    Out[6]: 0

Note how line that pexpect expects is written as ``[Pp]assword``. This is a
regex that describes a ``password`` or ``Password`` string. That is, ``expect``
method can be used to pass a regex as an argument.

Method ``expect`` returned number 0 as a result of the work. This number
indicates that a match has been found and that this element with index zero.
Index appears here because you can transfer a list of strings. For example,
you can transfer a list with two elements:

.. code:: python

    In [7]: ssh = pexpect.spawn('ssh cisco@192.168.100.1')

    In [8]: ssh.expect(['password', 'Password'])
    Out[8]: 1

Note that it now returns 1. This means that ``Password`` word matched.

Now you can send password using ``sendline`` method:

.. code:: python

    In [9]: ssh.sendline('cisco')
    Out[9]: 6

Method ``sendline`` sends a string, automatically adds a new line character
to it based on the value of ``os.linesep`` and then returns a number
indicating how many bytes were written.

.. note::

    Pexpect has several methods for sending commands, not just sendline.

To get into enable mode expect-sendline cycle repeats:

.. code:: python

    In [10]: ssh.expect('[>#]')
    Out[10]: 0

    In [11]: ssh.sendline('enable')
    Out[11]: 7

    In [12]: ssh.expect('[Pp]assword')
    Out[12]: 0

    In [13]: ssh.sendline('cisco')
    Out[13]: 6

    In [14]: ssh.expect('[>#]')
    Out[14]: 0

Now we can send a command:

.. code:: python

    In [15]: ssh.sendline('sh ip int br')
    Out[15]: 13

After sending the command, pexpect must be told until what point to read the output.
We specify that it should read untill ``#``:

.. code:: python

    In [16]: ssh.expect('#')
    Out[16]: 0

Command output is in ``before`` attribute:

.. code:: python

    In [17]: ssh.before
    Out[17]: b'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                19.1.1.1        YES NVRAM  up                    up      \r\nEthernet0/3                192.168.230.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nR1'

Since the result is displayed as a sequence of bytes you should convert
it to a string:

.. code:: python

    In [18]: show_output = ssh.before.decode('utf-8')

    In [19]: print(show_output)
    sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    Ethernet0/2                19.1.1.1        YES NVRAM  up                    up
    Ethernet0/3                192.168.230.1   YES NVRAM  up                    up
    Ethernet0/3.100            10.100.0.1      YES NVRAM  up                    up
    Ethernet0/3.200            10.200.0.1      YES NVRAM  up                    up
    Ethernet0/3.300            10.30.0.1       YES NVRAM  up                    up
    R1

Session ends with a ``close`` call:

.. code:: python

    In [20]: ssh.close()

Special characters in shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pexpect does not interpret special shell characters such as ``>``,
``|``, ``*``.

For example, in order make command ``ls -ls | grep SUMMARY`` work, shell
must be run as follows:

.. code:: python

    In [1]: import pexpect

    In [2]: p = pexpect.spawn('/bin/bash -c "ls -ls | grep pexpect"')

    In [3]: p.expect(pexpect.EOF)
    Out[3]: 0

    In [4]: print(p.before)
    b'4 -rw-r--r-- 1 vagrant vagrant 3203 Jul 14 07:15 1_pexpect.py\r\n'

    In [5]: print(p.before.decode('utf-8'))
    4 -rw-r--r-- 1 vagrant vagrant 3203 Jul 14 07:15 1_pexpect.py

pexpect.EOF
~~~~~~~~~~~

In the previous example we met pexpect.EOF.

.. note::

    EOF — end of file

This is a special value that allows you to react to the end of a command or
session that has been run in spawn.

When calling ``ls -ls`` command, pexpect does not receive an interactive
session. Command is simply executed and that ends its work.

Therefore, if you run this command and set prompt in ``expect``, there is an error:

.. code:: python

    In [5]: p = pexpect.spawn('/bin/bash -c "ls -ls | grep SUMMARY"')

    In [6]: p.expect('nattaur')
    ---------------------------------------------------------------------------
    EOF                                       Traceback (most recent call last)
    <ipython-input-9-9c71777698c2> in <module>()
    ----> 1 p.expect('nattaur')
    ...

If EOF passed to ``expect``, there will be no error.

Method ``pexpect.expect``
~~~~~~~~~~~~~~~~~~~~

In ``pexpect.expect`` as a value can be used:

* regex
* EOF - this template allows you to react to EOF exception
* TIMEOUT - timeout exception (default timeout = 30 seconds)
* compiled regex

Another very useful feature of ``pexpect.expect`` is that you can pass
not a single value, but a list.

For example:

.. code:: python

    In [7]: p = pexpect.spawn('/bin/bash -c "ls -ls | grep netmiko"')

    In [8]: p.expect(['py3_convert', pexpect.TIMEOUT, pexpect.EOF])
    Out[8]: 2

Here are some important points:

* when pexpect.expect is called with a list, you can specify different expected strings 
* apart strings, exceptions also can be specified
* pexpect.expect returns number of element that matched

  * in this case number 2 because EOF exception is number two in the list  

* with this format you can make branches in the program depending on the element which had a match

Example of pexpect use
----------------------------

Example of using pexpect when connecting to equipment and passing
show command (file 1_pexpect.py):

.. code:: python

    import pexpect
    import re
    from pprint import pprint


    def send_show_command(ip, username, password, enable, commands, prompt="#"):
        with pexpect.spawn(f"ssh {username}@{ip}", timeout=10, encoding="utf-8") as ssh:
            ssh.expect("[Pp]assword")
            ssh.sendline(password)
            enable_status = ssh.expect([">", "#"])
            if enable_status == 0:
                ssh.sendline("enable")
                ssh.expect("[Pp]assword")
                ssh.sendline(enable)
                ssh.expect(prompt)

            ssh.sendline("terminal length 0")
            ssh.expect(prompt)

            result = {}
            for command in commands:
                ssh.sendline(command)
                match = ssh.expect([prompt, pexpect.TIMEOUT, pexpect.EOF])
                if match == 1:
                    print(
                        f"Symbol {prompt} is not found in output. Resulting output is written to 
                        dictionary")
                if match == 2:
                    print("Connection was terminated by server")
                    return result
                else:
                    output = ssh.before
                    result[command] = output.replace("\r\n", "\n")
            return result


    if __name__ == "__main__":
        devices = ["192.168.100.1", "192.168.100.2", "192.168.100.3"]
        commands = ["sh clock", "sh int desc"]
        for ip in devices:
            result = send_show_command(ip, "cisco", "cisco", "cisco", commands)
            pprint(result, width=120)

This part of function is responsible for switching to enable mode:

.. code:: python

    enable_status = ssh.expect([">", "#"])
    if enable_status == 0:
        ssh.sendline("enable")
        ssh.expect("[Pp]assword")
        ssh.sendline(enable)
        ssh.expect(prompt)

If ``ssh.expect([">", "#"])`` does not return index 0, it means that connection
was not switched to enable mode automaticaly and it should be done separately.
If index 1 is returned, then we are already in enable mode, for example,
because device is configured with privilege 15.

Another interesting point about this function:

.. code:: python

    for command in commands:
        ssh.sendline(command)
        match = ssh.expect([prompt, pexpect.TIMEOUT, pexpect.EOF])
        if match == 1:
            print(
                f"Symbol {prompt} is not found in output. Resulting output is written to dictionary"
            )
        if match == 2:
            print("Connection was terminated by server")
            return result
        else:
            output = ssh.before
            result[command] = output.replace("\r\n", "\n")
    return result

Here commands are sent in turn and ``expect`` waits for three options:
prompt, timeout or EOF.
If ``expect`` method didn't catch ``#``, value 1 will be returned and in this
case a message is displayed, that symbol was not found. But in both cases,
when a match is found or timeout the resulting output is written to dictionary.
Thus, you can see what was received from device, even if prompt is not found.

Output after script execution:

::

    {'sh clock': 'sh clock\n*13:13:47.525 UTC Sun Jul 19 2020\n',
     'sh int desc': 'sh int desc\n'
                    'Interface                      Status         Protocol Description\n'
                    'Et0/0                          up             up       \n'
                    'Et0/1                          up             up       \n'
                    'Et0/2                          up             up       \n'
                    'Et0/3                          up             up       \n'
                    'Lo22                           up             up       \n'
                    'Lo33                           up             up       \n'
                    'Lo45                           up             up       \n'
                    'Lo55                           up             up       \n'}
    {'sh clock': 'sh clock\n*13:13:50.450 UTC Sun Jul 19 2020\n',
     'sh int desc': 'sh int desc\n'
                    'Interface                      Status         Protocol Description\n'
                    'Et0/0                          up             up       \n'
                    'Et0/1                          up             up       \n'
                    'Et0/2                          admin down     down     \n'
                    'Et0/3                          admin down     down     \n'
                    'Lo0                            up             up       \n'
                    'Lo9                            up             up       \n'
                    'Lo19                           up             up       \n'
                    'Lo33                           up             up       \n'
                    'Lo100                          up             up       \n'}
    {'sh clock': 'sh clock\n*13:13:53.360 UTC Sun Jul 19 2020\n',
     'sh int desc': 'sh int desc\n'
                    'Interface                      Status         Protocol Description\n'
                    'Et0/0                          up             up       \n'
                    'Et0/1                          up             up       \n'
                    'Et0/2                          admin down     down     \n'
                    'Et0/3                          admin down     down     \n'
                    'Lo33                           up             up       \n'}

Working with pexpect without disabling commands pagination
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sometimes the output of a command is very large and cannot be read completely
or device is not makes it possible to disable pagination. In this case, a
slightly different approach is needed.

.. note::

    The same task will be repeated for other modules in this section.


Example of using pexpect to work with paginated output of show
command (1_pexpect_more.py file):

.. code:: python

    import pexpect
    import re
    from pprint import pprint


    def send_show_command(ip, username, password, enable, command, prompt="#"):
        with pexpect.spawn(f"ssh {username}@{ip}", timeout=10, encoding="utf-8") as ssh:
            ssh.expect("[Pp]assword")
            ssh.sendline(password)
            enable_status = ssh.expect([">", "#"])
            if enable_status == 0:
                ssh.sendline("enable")
                ssh.expect("[Pp]assword")
                ssh.sendline(enable)
                ssh.expect(prompt)

            ssh.sendline(command)
            output = ""

            while True:
                match = ssh.expect([prompt, "--More--", pexpect.TIMEOUT])
                page = ssh.before.replace("\r\n", "\n")
                page = re.sub(" +\x08+ +\x08+", "\n", page)
                output += page
                if match == 0:
                    break
                elif match == 1:
                    ssh.send(" ")
                else:
                    print("Error: timeout")
                    break
            output = re.sub("\n +\n", "\n", output)
            return output


    if __name__ == "__main__":
        devices = ["192.168.100.1", "192.168.100.2", "192.168.100.3"]
        for ip in devices:
            result = send_show_command(ip, "cisco", "cisco", "cisco", "sh run")
            with open(f"{ip}_result.txt", "w") as f:
                f.write(result)


Now after sending the command, expect() method waits for another
option ``--More--`` - sign, that there will be one more page further.
Since it's not known in advance how many pages will be in the output,
reading is performed in a loop ``while True``. Loop is interrupted if
prompt is met ``#`` or no prompt appears within 10 seconds or ``--More--``.

If ``--More--`` is met, pages are not over yet and you have to scroll through
the next one. In Cisco, you need to press space bar to do this (without new line).
Therefore, ``send`` method is used here, not ``sendline`` - sendline automatically
adds a new line character.

This string ``page = re.sub(" +\x08+ +\x08+", "\n", page)`` removes backspace
symbols which are around ``--More--`` so they don't end up in the final output.




