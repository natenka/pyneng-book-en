.. _unpacking_args:

Unpacking arguments
---------------------

In Python the expressions ``*args`` and ``**kwargs`` allow for another task - **unpacking arguments**.

So far we’ve called all functions manually. Hence, we passed on all the relevant arguments.

In reality, it is usually necessary to transfer data to the function programmatically. And often data comes in the form of a Python object.

Unpacking positional arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For example, when formatting strings you often need to pass multiple arguments to format() method. And often these arguments are already in the list or tuple. To transfer them to the format() method you have to use indexes:

.. code:: python

    In [1]: items = [1,2,3]

    In [2]: print('One: {}, Two: {}, Three: {}'.format(items[0], items[1], items[2]))
    One: 1, Two: 2, Three: 3

Instead, you can take advantage of unpacking argument and do this:

.. code:: python

    In [4]: items = [1,2,3]

    In [5]: print('One: {}, Two: {}, Three: {}'.format(*items))
    One: 1, Two: 2, Three: 3


Another example is config_interface function (func_config_interface_unpacking.py file):

.. code:: python

    In [8]: def config_interface(intf_name, ip_address, mask):
        ..:     interface = f'interface {intf_name}'
        ..:     no_shut = 'no shutdown'
        ..:     ip_addr = f'ip address {ip_address} {mask}'
        ..:     result = [interface, no_shut, ip_addr]
        ..:     return result
        ..:


The function expects such arguments:

* intf_name - interface name
* ip_address - IP address
* mask - subnet mask

Function returns a list of strings to configure the interface:

.. code:: python


    In [9]: config_interface('Fa0/1', '10.0.1.1', '255.255.255.0')
    Out[9]: ['interface Fa0/1', 'no shutdown', 'ip address 10.0.1.1 255.255.255.0']

    In [11]: config_interface('Fa0/10', '10.255.4.1', '255.255.255.0')
    Out[11]: ['interface Fa0/10', 'no shutdown', 'ip address 10.255.4.1 255.255.255.0']


Suppose you call a function and pass it information that has been obtained from another source, for example from the database.

For example, interfaces_info list contains parameters for configuring interfaces:

.. code:: python

    In [14]: interfaces_info = [['Fa0/1', '10.0.1.1', '255.255.255.0'],
        ...:                    ['Fa0/2', '10.0.2.1', '255.255.255.0'],
        ...:                    ['Fa0/3', '10.0.3.1', '255.255.255.0'],
        ...:                    ['Fa0/4', '10.0.4.1', '255.255.255.0'],
        ...:                    ['Lo0', '10.0.0.1', '255.255.255.255']]
        ...:


If you go through the list in the loop and pass the nested list as a function argument, an error will occur:

.. code:: python

    In [15]: for info in interfaces_info:
        ...:     print(config_interface(info))
        ...:
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-15-d34ced60c796> in <module>
          1 for info in interfaces_info:
    ----> 2     print(config_interface(info))
          3

    TypeError: config_interface() missing 2 required positional arguments: 'ip_address' and 'mask'

The error is quite logical: the function expects three arguments and it is given 1 argument - a list.

In such a situation it is necessary to unpack the arguments. Just add ``*`` before passing the list as an argument and there is no error anymore:

.. code:: python

    In [16]: for info in interfaces_info:
        ...:     print(config_interface(*info))
        ...:
    ['interface Fa0/1', 'no shutdown', 'ip address 10.0.1.1 255.255.255.0']
    ['interface Fa0/2', 'no shutdown', 'ip address 10.0.2.1 255.255.255.0']
    ['interface Fa0/3', 'no shutdown', 'ip address 10.0.3.1 255.255.255.0']
    ['interface Fa0/4', 'no shutdown', 'ip address 10.0.4.1 255.255.255.0']
    ['interface Lo0', 'no shutdown', 'ip address 10.0.0.1 255.255.255.255']


Python will unpack the *info* list itself and transfer list elements to the function as arguments.

.. note::
    Tuple can also be unpacked in this way.

Unpacking keyword alrguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similarly, you can unpack dictionary to pass it as keyword arguments.

Check_passwd function (func_check_pass_optional_param_2.py file):

.. code:: python

    In [19]: def check_passwd(username, password, min_length=8, check_username=True):
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


List of dictionaries ``username_passwd`` where username and password are specified:

.. code:: python

    In [20]: username_passwd = [{'username': 'cisco', 'password': 'cisco'},
        ...:                    {'username': 'nata', 'password': 'natapass'},
        ...:                    {'username': 'user', 'password': '123456789'}]

If you pass dictionary to check_passwd function, there is an error:

.. code:: python

    In [21]: for data in username_passwd:
        ...:     check_passwd(data)
        ...:
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-21-ad848f85c77f> in <module>
          1 for data in username_passwd:
    ----> 2     check_passwd(data)
          3

    TypeError: check_passwd() missing 1 required positional argument: 'password'


The error is because the function has taken dictionary as one argument and believes that it lacks only password argument.

If you add ``**`` пbefore passing a dictionary to function, the function will work properly:

.. code:: python

    In [22]: for data in username_passwd:
        ...:     check_passwd(**data)
        ...:
    Password is too short
    Password contains username
    Password for user user has passed all checks

    In [23]: for data in username_passwd:
        ...:     print(data)
        ...:     check_passwd(**data)
        ...:
    {'username': 'cisco', 'password': 'cisco'}
    Password is too short
    {'username': 'nata', 'password': 'natapass'}
    Password contains username
    {'username': 'user', 'password': '123456789'}
    Password for user user has passed all checks

Python unpacks dictionary and passes it to the function as keyword arguments. The  ``check_passwd(**data)`` is converted to a ``check_passwd(username='cisco', password='cisco')``.

