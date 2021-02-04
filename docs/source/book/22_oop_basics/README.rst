OOP basics
----------

-  Class - an element of a program that describes some data type. Class describes
   a template for creating objects, typically specifies variables of object and actions that can be performed on object.
-  Instance - an object that is a representative of a class.
-  Method - a function that is defined within a class and describes an action that class supports
-  Instance variable (sometimes instance
   attribute) - data that refer to an object
-  Class variable - data that refer to class and shared by all class instances
-  Instance attribute - variables and methods that refer to objects (instances) created on the basis of a class. Every object has its own copy of attributes.

A real-life OOP example:

-  Building project - it is a class
-  Particular house which was built according to project - instance
-  Features such as color of house, number of windows - instance variables (of this particular house)
-  House can be sold, repainted, repaired - methods


For example, when working with netmiko, the first thing to do was
create connection:

.. code:: python
    
    from netmiko import ConnectHandler

    device = {
        "device_type": "cisco_ios",
        "host": "192.168.100.1",
        "username": "cisco",
        "password": "cisco",
        "secret": "cisco",
    }

    ssh = ConnectHandler(**device)


The ``ssh`` variable is an object that represents the real
connection to equipment. Thanks to the type function, you can find out by
an instance what class is the ssh object:

.. code:: python

    In [3]: type(ssh)
    Out[3]: netmiko.cisco.cisco_ios.CiscoIosSSH

``ssh`` has its own methods and variables that depend on the state
the current object. For example, the instance variable ``ssh.host``
is available for every instance of the class ``netmiko.cisco.cisco_ios.CiscoIosSSH``
and returns IP address or hostname, whichever is specified in the device dictionary:

.. code:: python

    In [4]: ssh.host
    Out[4]: '192.168.100.1'


Method ``send_command`` executes a command on the network device:

.. code:: python

    In [5]: ssh.send_command("sh clock")
    Out[5]: '*10:08:50.654 UTC Tue Feb 2 2021'


The ``enable`` method goes into enable mode and the ``ssh`` object
saves state: before and after the transition, a different prompt is visible:

.. code:: python

    In [6]: ssh.find_prompt()
    Out[6]: 'R1>'

    In [7]: ssh.enable()
    Out[7]: 'enable\r\nPassword: \r\nR1#'

    In [8]: ssh.find_prompt()
    Out[8]: 'R1#'


This example illustrates important aspects of OOP: data integration, data handling and state preservation.

Until now, when writing code, data and actions have been separated. Most often,
actions are described as functions, and data is passed as arguments to these functions.
When a class is created, data and actions are combined. Of course, these data
and actions are linked. That is, those actions that are characteristic of an
object of this type, and not some arbitrary actions, become class methods.


For example, in an class instance ``str``, all methods refer to working with this string:

.. code:: python

    In [10]: s = 'string'

    In [11]: s.upper()
    Out[11]: 'STRING'

    In [12]: s.center(20, '=')
    Out[12]: '=======string======='


.. note::

    Class does not have to store a state - string is immutable data type
    and all methods return new strings and do not change the original string.

Above, the following syntax is used when referring to instance attributes
(variables and methods): ``objectname.attribute``. This entry  ``s.lower()``
means: call ``lower`` method on ``s`` object. Calling methods and variables
is the same, but to call a method you have to add parentheses and pass all
necessary arguments.

Everything described has been used repeatedly in the book but now we will deal
with formal terminology.

