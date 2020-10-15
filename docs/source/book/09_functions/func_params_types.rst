Function parameter types
-----------------------

When creating a function you can specify which arguments must be passed and which must not. Accordingly, a function can be created with:

* **required parameters**
* **optional parameters** (with default values)

Required parameters
~~~~~~~~~~~~~~~~~~~~~~

**Required parameters** - determine which arguments must be passed to functions. At the same time, they need to be passed exactly as many as  parameters of function are specified (you cannot specify more or less)

Function with mandatory parameters (func_params_types.py file):

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


Function check_passwd() expects two arguments: username and password.

Function checks password and returns False if checks fail and True if password passed all checks:

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


Optional parameters (default parameters)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When creating a function you can specify default value for parameter in this way: ``def check_passwd(username, password, min_length=8)``. In this case, min_length option is specified with default value and may not be passed when a function is called.


Example of a check_passwd() function with default parameter (func_check_passwd_optional_param.py file):

.. code:: python

    In [6]: def check_passwd(username, password, min_length=8):
       ...:     if len(password) < min_length:
       ...:         print('Password is too short')
       ...:         return False
       ...:     elif username in password:
       ...:         print('Password contains username')
       ...:         return False
       ...:     else:
       ...:         print(f'Password for user {username} has passed all checks')
       ...:         return True
       ...:

Since min_length parameter has a default value the corresponding argument can be omitted when a function is called if default value fits you:

.. code:: python

    In [7]: check_passwd('nata', '12345')
    Password is too short
    Out[7]: False


If you want to change default value:

.. code:: python

    In [8]: check_passwd('nata', '12345', 3)
    Password for user nata has passed all checks
    Out[8]: True

