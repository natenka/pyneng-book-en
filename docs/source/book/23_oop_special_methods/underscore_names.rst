Underscore in names
----------------------

In Python, underscore at the beginning or at the end of a name indicates special names. Most often it’s just an arrangement, but sometimes it actually affects object behavior.


One underscore before name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One underscore before method name indicates that method is an internal feature of the implementation and it should not be used directly.

For example, CiscoSSH class uses paramiko to connect to equipment:

.. code:: python

    import time
    import paramiko


    class CiscoSSH:
        def __init__(self, ip, username, password, enable, disable_paging=True):
            self.client = paramiko.SSHClient()
            self.client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
            self.client.connect(
                hostname=ip,
                username=username,
                password=password,
                look_for_keys=False,
                allow_agent=False)

            self.ssh = self.client.invoke_shell()
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


After creating an instance of the class, not only send_show_command method is available but also *client* and *ssh* attributes (3rd line is tab tips in ipython)::

.. code:: python

    In [2]: r1 = CiscoSSH('192.168.100.1', 'cisco', 'cisco', 'cisco')

    In [3]: r1.
                client
                send_show_command()
                ssh


If you want to specify that *client* and *ssh* are internal attributes that are needed for class operation but are not intended for the user, you need to underscore name below:

.. code:: python

    class CiscoSSH:
        def __init__(self, ip, username, password, enable, disable_paging=True):
            self._client = paramiko.SSHClient()
            self._client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

            self._client.connect(
                hostname=ip,
                username=username,
                password=password,
                look_for_keys=False,
                allow_agent=False)

            self._ssh = self._client.invoke_shell()
            self._ssh.send('enable\n')
            self._ssh.send(enable + '\n')
            if disable_paging:
                self._ssh.send('terminal length 0\n')
            time.sleep(1)
            self._ssh.recv(1000)

        def send_show_command(self, command):
            self._ssh.send(command + '\n')
            time.sleep(2)
            result = self._ssh.recv(5000).decode('ascii')
            return result


.. note::

    Often such methods and attributes are called private but this does not mean that methods and variables are not available to the user.




Two underscores before name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Two underscores before method name are not used simply as an agreement. Such names are transformed into format "name of class + name of method". This allows the creation of unique methods and attributes of classes.

This conversion is only performed if less than two underscores endings or no underscores.

.. code:: python

    In [14]: class Switch(object):
        ...:     __quantity = 0
        ...:
        ...:     def __configure(self):
        ...:         pass
        ...:

    In [15]: dir(Switch)
    Out[15]:
    ['_Switch__configure', '_Switch__quantity', ...]

Although methods were created without ``_Switch``, it was added.

If you create a subclass then ``__configure`` method will not rewrite parent class method Switch:

.. code:: python

    In [16]: class CiscoSwitch(Switch):
        ...:     __quantity = 0
        ...:     def __configure(self):
        ...:         pass
        ...:

    In [17]: dir(CiscoSwitch)
    Out[17]:
    ['_CiscoSwitch__configure', '_CiscoSwitch__quantity', '_Switch__configure', '_Switch__quantity', ...]

Two underscores before and after name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Thus, special variables and methods are denoted.

For example, Python module has such special variables:

* ``__name__`` - this variable is equal to ``__main__`` when the script runs directly and is equal to module name when imported
* ``__file__`` - this variable is equal  to name of the script that was run directly and equals to complete path to module when it is imported

Variable ``__name__`` is most commonly used to indicate that a certain part of code must be executed only when module is called directly:

.. code:: python


    def multiply(a, b):

        return a * b

    if __name__ == '__main__':
        print(multiply(3, 5))

And ``__file__`` variable can be useful in determining the current path to script file:

.. code:: python

    import os

    print('__file__', __file__)
    print(os.path.abspath(__file__))

The output will be:

::

    __file__ example2.py
    /home/vagrant/repos/tests/example2.py

Python also denotes special methods. These methods are called when using Python functions and operators and allow to implement a certain functionality.

As a rule, such methods need not be called directly. But for example, when creating your own class it may be necessary to describe such method in order to object can support some operations in Python.

For example, in order to get length of an object it must support  ``__len__`` method.

