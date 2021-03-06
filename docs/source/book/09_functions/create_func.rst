Creation of functions
----------------

Creation of function:

* functions are created with a reserved word ``def``
* ``def`` followed by function name and parentheses
* parameters that function accepts inside parentheses
* after parentheses goes colon and from a new line with indent there is a block of code that function executes
* optionally, the first line can be ``docstring``
* function can use ``return`` operator

  * it is used to terminate and exit a function
  * most often ``return`` operator returns some value

.. note::

    Function code used in this subsection can be copied from create_func file.

Example of function:

.. code:: python

    In [1]: def configure_intf(intf_name, ip, mask):
       ...:     print('interface', intf_name)
       ...:     print('ip address', ip, mask)
       ...:

Function ``configure_intf`` creates an interface configuration with specified name and IP address. 
Function has three parameters: intf_name, ip, mask. When function is called the real data will replace these parameters.

.. note::
    When function is created, it does nothing yet. Actions listed in it will
    be executed only when you call function. This is something like ACL in
    network equipment: when creating ACL in configuration, it does nothing until it is applied.
    
Function call
~~~~~~~~~~~~~

When calling a function you must specify its name and pass arguments if necessary.

.. note::
    Parameters are variables that are used to create a function.
    Arguments are the actual values (data) that are passed to functions when called.

Function ``configure_intf`` expects three values when called because it was created with three parameters:

.. code:: python

    In [2]: configure_intf('F0/0', '10.1.1.1', '255.255.255.0')
    interface F0/0
    ip address 10.1.1.1 255.255.255.0

    In [3]: configure_intf('Fa0/1', '94.150.197.1', '255.255.255.248')
    interface Fa0/1
    ip address 94.150.197.1 255.255.255.248

The current version of the ``configure_intf`` function prints commands
to a standard output, commands can be seen but the result of function cannot be saved to a variable.

For example, ``sorted`` function does not simply print the sorting result to standard output
stream but **returns** it, so it can be saved to variable in this way:

.. code:: python

    In [4]: items = [40, 2, 0, 22]

    In [5]: sorted(items)
    Out[5]: [0, 2, 22, 40]

    In [6]: sorted_items = sorted(items)

    In [7]: sorted_items
    Out[7]: [0, 2, 22, 40]

.. note::

    Note string ``Out[5]`` in ipython: this is how ipython shows that
    the function/method is returning something and shows what it returns.

If you try to write the result of the ``configure_intf`` function to
a variable, the value of the variable will be None::

.. code:: python

    In [8]: result = configure_intf('Fa0/0', '10.1.1.1', '255.255.255.0')
    interface Fa0/0
    ip address 10.1.1.1 255.255.255.0

    In [9]: print(result)
    None

For a function to return a value, use ``return`` operator.

Operator return
~~~~~~~~~~~~~~~

Operator ``return`` is used to return a value,
and at the same time it exits the function.
Function can return any Python object. By default, function always returns ``None``.

In order for ``configure_intf`` function to return a value that can then
be assigned to a variable, you must use ``return`` operator:

.. code:: python

    In [10]: def configure_intf(intf_name, ip, mask):
        ...:     config = f'interface {intf_name}\nip address {ip} {mask}'
        ...:     return config
        ...:

    In [11]: result = configure_intf('Fa0/0', '10.1.1.1', '255.255.255.0')

    In [12]: print(result)
    interface Fa0/0
    ip address 10.1.1.1 255.255.255.0

    In [13]: result
    Out[13]: 'interface Fa0/0\nip address 10.1.1.1 255.255.255.0'


Now the result variable contains a line with commands to configure interface.
In real life, function will almost always return some value.

Another important aspect of ``return`` operator is that after ``return``
the function closes, meaning that the expressions that follow ``return`` are not executed.

For example, in function below the line «Configuration is ready» will not be displayed because it stands after ``return``:

.. code:: python

    In [14]: def configure_intf(intf_name, ip, mask):
        ...:     config = f'interface {intf_name}\nip address {ip} {mask}'
        ...:     return config
        ...:     print('Configuration is ready')
        ...:

    In [15]: configure_intf('Fa0/0', '10.1.1.1', '255.255.255.0')
    Out[15]: 'interface Fa0/0\nip address 10.1.1.1 255.255.255.0'

Function can return multiple values. In this case, they are separated by a comma after ``return`` operator. In fact, function returns tuple:

.. code:: python

    In [16]: def configure_intf(intf_name, ip, mask):
        ...:     config_intf = f'interface {intf_name}\n'
        ...:     config_ip = f'ip address {ip} {mask}'
        ...:     return config_intf, config_ip
        ...:

    In [17]: result = configure_intf('Fa0/0', '10.1.1.1', '255.255.255.0')

    In [18]: result
    Out[18]: ('interface Fa0/0\n', 'ip address 10.1.1.1 255.255.255.0')

    In [19]: type(result)
    Out[19]: tuple

    In [20]: intf, ip_addr = configure_intf('Fa0/0', '10.1.1.1', '255.255.255.0')

    In [21]: intf
    Out[21]: 'interface Fa0/0\n'

    In [22]: ip_addr
    Out[22]: 'ip address 10.1.1.1 255.255.255.0'


Documentation (docstring)
~~~~~~~~~~~~~~~~~~~~~~~~

The first line in function definition is docstring, documentation string. This is a comment that is used to describe a function:

.. code:: python

    In [23]: def configure_intf(intf_name, ip, mask):
        ...:     '''
        ...:     Fucntion generates interface configuration
        ...:     '''
        ...:     config_intf = f'interface {intf_name}\n'
        ...:     config_ip = f'ip address {ip} {mask}'
        ...:     return config_intf, config_ip
        ...:

    In [24]: configure_intf?
    Signature: configure_intf(intf_name, ip, mask)
    Docstring: Fucntion generates interface configuration
    File:      ~/repos/pyneng-examples-exercises/examples/06_control_structures/<ipython-input-23-2b2bd970db8f>
    Type:      function



It is best to write short comments that describe function. For example,
describe what function expects to input, what type of arguments should be
and what will be the output. Besides, it is better to write a couple of
sentences about what function does. This will help when in a month or two
you will be trying to understand what function you wrote is doing.

