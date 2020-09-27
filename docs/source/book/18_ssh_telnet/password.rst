Password input
-----------

During manual connection to device the password is also manually entered.

When automating the connection it is necessary to decide how the password will be transmitted:

* Request password at start of the script and read user input. Disadvantage is that you can see which characters the user is typing
* Write login and password in some file (it’s not secure).

As a rule, the same user uses the same login and password to connect to devices. And usually it’s enough to request login and password at the start of the script and then use them to connect to different devices.

Unfortunately, if you use ``input()`` the typed password will be visible. But it is desirable that no characters are displayed when entering a password.

Module getpass
~~~~~~~~~~~~~~

Module getpass allows you to request a password without displaying input characters:

.. code:: python

    In [1]: import getpass

    In [2]: password = getpass.getpass()
    Password:

    In [3]: print(password)
    testpass

Environment variables
~~~~~~~~~~~~~~~~~~~~

Another way to store a password (or even a username) is by environment variables.

For example, login and password are written in variables:

::

    $ export SSH_USER=user
    $ export SSH_PASSWORD=userpass

And then Python reads values to variables in the script:

.. code:: python

    import os

    USERNAME = os.environ.get('SSH_USER')
    PASSWORD = os.environ.get('SSH_PASSWORD')

