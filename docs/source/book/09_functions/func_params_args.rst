Function parameters and arguments
#############################

The purpose of creating a function is typically to take a piece of code that performs a particular task to a separate object. This allows you to use this piece of code multiple times without having to re-create it in program.

Typically, a function must perform some actions with input values and produce an output.

When working with functions it is important to distinguish:

-  **parameters** - variables that are used when creating a function.
-  **arguments** - actual values (data) that are passed to function when called.

For a function to receive incoming values, it must be created with parameters (func_check_passwd.py file):

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

In this case, function has two parameters: username and password.

Function checks password and returns False if checks fail and True if password passed checks:

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


When defining a function in this way it is necessary to pass both arguments. If only one argument is passed, there is an error:

.. code:: python

    In [5]: check_passwd('nata')
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-5-e07773bb4cc8> in <module>
    ----> 1 check_passwd('nata')

    TypeError: check_passwd() missing 1 required positional argument: 'password'


Similarly, an error will occur if three or more arguments are passed.

.. toctree::
   :maxdepth: 1

   func_params_types.rst
   func_args_types.rst
   func_args_var.rst      
   func_unpacking_args.rst

