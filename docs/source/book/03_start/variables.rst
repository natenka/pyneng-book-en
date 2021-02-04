Variables
~~~~~~~~~~

Variables in Python do not require variable type declaration (since Python is a language with dynamic typing) and they are references to a memory area. Variable naming rules:

-  Name of variable can consist only of letters, digits and an underscore
-  Name cannot start with a digit
-  Name cannot contain special characters @, $, %

An example of creating variables in Python:

.. code:: python

    In [1]: a = 3

    In [2]: b = 'Hello'

    In [3]: c, d = 9, 'Test'

    In [4]: print(a,b,c,d)
    3 Hello 9 Test

Note that Python does not need to specify that "a" is a number, and "b" is a string.

Variables are references to memory area. This can be demonstrated by using ``id()`` which shows object ID:

.. code:: python

    In [5]: a = b = c = 33

    In [6]: id(a)
    Out[6]: 31671480

    In [7]: id(b)
    Out[7]: 31671480

    In [8]: id(c)
    Out[8]: 31671480

In this example you can see that all three names refer to the same identifier, so it is the same object to which three references "a", "b" and "c" point. Concerning numbers Python has one feature that can be slightly misunderstood: numbers from -5 to 256 are pre-created and stored in an array (list). Therefore, when you create a number from this range you actually create a reference to number in generated array.

.. note::
    This feature is specific to implementation of CPython which is discussed in book

This can be verified as follows:

.. code:: python

    In [9]: a = 3

    In [10]: b = 3

    In [11]: id(a)
    Out[11]: 4400936168

    In [12]: id(b)
    Out[12]: 4400936168

    In [13]: id(3)
    Out[13]: 4400936168

Note that ``a``, ``b`` and number ``3`` have identical identifiers. 
They are all references to an existing number in the list.

If you do the same with number more than 256, all identifiers will be different:

.. code:: python

    In [14]: a = 500

    In [15]: b = 500

    In [16]: id(a)
    Out[16]: 140239990503056

    In [17]: id(b)
    Out[17]: 140239990503032

    In [18]: id(500)
    Out[18]: 140239990502960

However, if you assign variables to each other, identifiers are all the same (in this variant ``a``, ``b`` and ``c``
are referring to the same object):

.. code:: python

    In [19]: a = b = c = 500

    In [20]: id(a)
    Out[20]: 140239990503080

    In [21]: id(b)
    Out[21]: 140239990503080

    In [22]: id(c)
    Out[22]: 140239990503080

Variable names
^^^^^^^^^^^^^^^^

Variable names should not overlap with names of operators and modules or other reserved words. Python has recommendations for naming functions, classes and variables:

-  variable names are usually written in lowercase or in uppercase (e.g., DB_NAME, db_name)
-  function names are written in lowercase, with underline between words (for example get_names)
-  class names are given with capital letters without spaces, it is called CamelCase (for example, CiscoSwitch)

