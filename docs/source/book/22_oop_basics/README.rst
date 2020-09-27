OOP basics
----------

-  Class - an element of a program that describes some data type. Class describes a template for creating objects, typically specifies variables of object and actions that can be performed on object.
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

Consider a practical example of OOP use.

In section "18. Working with databases" the first thing to do to work with database - connect to it:

.. code:: python

    In [1]: import sqlite3

    In [2]: conn = sqlite3.connect('dhcp_snooping.db')

``conn`` variable - an object that represents a real database connection. Using type() function you can find out which class instance the ``conn`` object belongs to:

.. code:: python

    In [3]: type(conn)
    Out[3]: sqlite3.Connection

``conn`` has its own methods and variables that depend on the state of current object. For example, conn.in_transaction instance variable is available in each instance of sqlite3.Connection class and returns True or False depending on whether all changes are commited:

.. code:: python

    In [15]: conn.in_transaction
    Out[15]: False

Method execute() executes SQL command:

.. code:: python

    In [19]: query = 'insert into dhcp (mac, ip, vlan, interface) values (?, ?, ?, ?)'

    In [5]: conn.execute(query, ('0000.1111.7777', '10.255.1.1', '10', 'Gi0/7'))
    Out[5]: <sqlite3.Cursor at 0xb57328a0>

``conn`` object saves the state: now instance variable conn.in_transaction returns True:

.. code:: python

    In [6]: conn.in_transaction
    Out[6]: True

After calling commit() method, it is again False:

.. code:: python

    In [7]: conn.commit()

    In [8]: conn.in_transaction
    Out[8]: False

This example illustrates important aspects of OOP: data integration, data handling and state preservation.

So far in code writing, data and actions on data have been separated. Most often, actions are described as functions and data are transmitted as arguments to these functions. When creating a class, data and actions are combined. Of course, these data and actions are connected. That is, class methods become those actions that are specific to this type of object, not some arbitrary action.

For example, in an class instance **str**, all methods refer to working with this string:

.. code:: python

    In [10]: s = 'string'

    In [11]: s.upper()
    Out[11]: 'STRING'

    In [12]: s.center(20, '=')
    Out[12]: '=======string======='


.. note::

    By example with a string, it is clear that class does not have to store a state - string is immutable data type and all methods return new strings and do not change the original string.

Above, the following syntax is used when referring to instance attributes (variables and methods): ``objectname.attribute``. This entry 
``s.lower()`` means: invoke lower() method on **s** object. Invoking methods and variables is the same, but to call a method you have to add brackets and pass all necessary arguments.

Everything described has been used repeatedly in the book but now we will deal with formal terminology.

