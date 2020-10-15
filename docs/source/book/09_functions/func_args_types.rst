Function argument types
-----------------------

When a function is called the arguments can be passed in two ways:

* as **positional** - passed in the same order in which they are defined at creation of function. That is, the order of argument transfer determines what value each argument will get.
* as **keyword** - passed with argument name and its value. In such a case, arguments can be stated in any order as their name is clearly indicated.

Positional and keyword arguments can be mixed when calling a function. That is, it is possible to use both methods when passing arguments of the same function. In this process, Positional arguments must be indicated before keyword arguments.

Look at different ways to pass arguments using check_passwd (func_check_check_passwd_optional_param.py file):

.. code:: python

    In [1]: def check_passwd(username, password):
       ...:     if len(password) < 8:
       ...:         print('Password is too short')
       ...:         return False
       ...:     elif username in password:
       ...:         print('Password contains username')
       ...:         return False
       ...:     else:
       ...:         print(f'Password for user {username} has passed all checks')
       ...:         return True
       ...:

Positional argument
~~~~~~~~~~~~~~~~~~~~~

Positional arguments when calling a function must be passed in the correct order (therefore they are called positional arguments).

.. code:: python

    In [2]: check_passwd('nata', '12345')
    Password is too short
    Out[2]: False

    In [3]: check_passwd('nata', '12345lsdkjflskfdjsnata')
    Password contains username
    Out[3]: False

    In [4]: check_passwd('nata', '12345lsdkjflskfdjs')
    Password for user nata has passed all checks
    Out[4]: True

If you swap arguments when calling a function the error will likely occur depending on function.

Keyword arguments
~~~~~~~~~~~~~~~~~~

**Keyword arguments**:

* are passed with name of argument
* thus they can be passed in any order

If both arguments are keyword, they can be passed in any order:

.. code:: python

    In [9]: check_passwd(password='12345', username='nata', min_length=4)
    Password for user nata has passed all checks
    Out[9]: True


.. warning::
    Please note that first there should always be positional arguments and then keyword arguments.

If you do the opposite, there’s an error:

.. code:: python

    In [10]: check_passwd(password='12345', username='nata', 4)
      File "<ipython-input-10-7e8246b6b402>", line 1
        check_passwd(password='12345', username='nata', 4)
                                                       ^
    SyntaxError: positional argument follows keyword argument


But in that combination it works:

.. code:: python

    In [11]: check_passwd('nata', '12345', min_length=3)
    Password for user nata has passed all checks
    Out[11]: True

In real life, it is often better to specify flags (parameters with True/False values) or numerical values as a keyword argument. If you set a good name for the parameter you can immediately know by its name what it does.

For example, you can add a flag that will control whether or not a username should be checked in password:

.. code:: python

    In [12]: def check_passwd(username, password, min_length=8, check_username=True):
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

By default, flag is True which means check should be done:

.. code:: python

    In [14]: check_passwd('nata', '12345nata', min_length=3)
    Password contains username
    Out[14]: False

    In [15]: check_passwd('nata', '12345nata', min_length=3, check_username=True)
    Password contains username
    Out[15]: False

If you specify a value equal to False the verification will not be performed:

.. code:: python

    In [16]: check_passwd('nata', '12345nata', min_length=3, check_username=False)
    Password for user nata has passed all checks
    Out[16]: True

