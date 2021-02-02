Example of using variable length keyword arguments and unpacking arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using variable length arguments and unpacking arguments you can transfer arguments between functions. Let me give you an example.

Function ``check_passwd`` (func_add_user_kwargs_example.py file):

.. code:: python

    In [1]: def check_passwd(username, password, min_length=8, check_username=True):
       ...:     if len(password) < min_length:
       ...:         print('Password is too short')
       ...:         return False
       ...:     elif check_username and username in password:
       ...:         print('Password contains username')
       ...:         return False
       ...:     else:
       ...:         print(f'Password for user {username} has passed all checks')
       ...:         return True
       ...:

Function checks password and returns True if password has passed verification and False if not.

Call function in ipython:

.. code:: python

    In [3]: check_passwd('nata', '12345', min_length=3)
    Password for user nata has passed all checks
    Out[3]: True

    In [4]: check_passwd('nata', '12345nata', min_length=3)
    Password contains username
    Out[4]: False

    In [5]: check_passwd('nata', '12345nata', min_length=3, check_username=False)
    Password for user nata has passed all checks
    Out[5]: True

    In [6]: check_passwd('nata', '12345nata', min_length=3, check_username=True)
    Password contains username
    Out[6]: False


We will create ``add_user_to_users_file`` function that requests password for
specified user, checks it and requests it again if password has not been checked
or writes user and password to file if password has been verified

.. code:: python

    In [7]: def add_user_to_users_file(user, users_filename='users.txt'):
       ...:     while True:
       ...:         passwd = input(f'Enter password for user {user}: ')
       ...:         if check_passwd(user, passwd):
       ...:             break
       ...:     with open(users_filename, 'a') as f:
       ...:         f.write(f'{user},{passwd}\n')
       ...:

    In [8]: add_user_to_users_file('nata')
    Enter password for user nata: natasda
    Password is too short
    Enter password for user nata: natasdlajsl;fjd
    Password contains username
    Enter password for user nata: salkfdjsalkdjfsal;dfj
    Password for user nata has passed all checks

    In [9]: cat users.txt
    nata,salkfdjsalkdjfsal;dfj

In this variant of add_user_to_users_file() function, it is not possible to regulate the minimum password length and whether to verify the presence of a username in password. In the following variant of add_user_to_users_file() function, these features are added:

.. code:: python

    In [5]: def add_user_to_users_file(user, users_filename='users.txt', min_length=8, check_username=True):
       ...:     while True:
       ...:         passwd = input(f'Enter password for user {user}: ')
       ...:         if check_passwd(user, passwd, min_length, check_username):
       ...:             break
       ...:     with open(users_filename, 'a') as f:
       ...:         f.write(f'{user},{passwd}\n')
       ...:

    In [6]: add_user_to_users_file('nata', min_length=5)
    Enter password for user nata: natas2342
    Password contains username
    Enter password for user nata: dlfjgkd
    Password for user nata has passed all checks

You can now specify min_length or check_username when calling a function. However,
it was necessary to repeat parameters of ``check_passwd`` function in defining of
``add_user_to_users_file`` function. This is not very good and when there are many
parameters it is just inconvenient, especially considering that check_passwd function
can have other parameters.

This happens quite often and Python has a common solution to this problem: all
arguments for internal function (in this case it is check_passwd) will be taken
in **kwargs. Then, when calling check_passwd() function they will be unpacked
into keyword arguments by the same  ``**kwargs`` syntax.

.. code:: python

    In [7]: def add_user_to_users_file(user, users_filename='users.txt', **kwargs):
       ...:     while True:
       ...:         passwd = input(f'Enter password for user {user}: ')
       ...:         if check_passwd(user, passwd, **kwargs):
       ...:             break
       ...:     with open(users_filename, 'a') as f:
       ...:         f.write(f'{user},{passwd}\n')
       ...:

    In [8]: add_user_to_users_file('nata', min_length=5)
    Enter password for user nata: alskfdjlksadjf
    Password for user nata has passed all checks

    In [9]: add_user_to_users_file('nata', min_length=5)
    Enter password for user nata: 345
    Password is too short
    Enter password for user nata: 309487538
    Password for user nata has passed all checks


In this variant you can add arguments to check_passwd() function without having
to duplicate them in add_user_to_users_file function.
