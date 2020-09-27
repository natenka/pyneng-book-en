Executable file
~~~~~~~~~~~~~~~~

In order for a file to be executable and not have to write "python" every time before calling a file, you need to:

* make the file executable (for Linux)
* the first line of the file should have ``#!/usr/bin/env python``
  or ``#!/usr/bin/env python3`` depending on which version of Python is used by default

Example of access_template_exec.py file:

.. code:: python

    #!/usr/bin/env python3

    access_template = ['switchport mode access',
                       'switchport access vlan {}',
                       'switchport nonegotiate',
                       'spanning-tree portfast',
                       'spanning-tree bpduguard enable']

    print('\n'.join(access_template).format(5))

After that:

::

    chmod +x access_template_exec.py

Now you can call file like this:

::

    $ ./access_template_exec.py

