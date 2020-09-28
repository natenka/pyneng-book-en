Python syntax
~~~~~~~~~~~~~~~~

The first thing that meets the eye when it comes to Python syntax is that indentation matters:

-  It determines which code enters the block;
-  When a block of code starts and ends.

Example of Python code:

.. code:: python

    a = 10
    b = 5

    if a > b:
        print("A greater than B")
        print(a - b)
    else:
        print("B is greater than or equal to A")
        print(b - a)

    print("End")

    def open_file(filename):
        print("Reading File", filename)
        with open(filename) as f:
            return f.read()
            print("Ready")

.. note::
    This code is shown for syntax demonstration. Although if/else construction has not yet been considered, it is likely that the meaning of code will be understood.

Python understands which lines refer to "if" on indentation basis.
Execution of a block ``if a > b`` ends when another string with the same indent as string ``if a > b`` appears. Similarly to block “else”. 
The second feature of Python is that some expressions must be followed by colon (for example, after ``if a > b`` and after ``else``).

Several rules and recommendations on indentation:

-  Tabs or spaces can be used as indents (it is better to use spaces or more precisely to configure editor so that Tab is 4 spaces - then when using Tab key, 4 spaces will be placed instead of 1 tab sign).
-  Number of spaces must be the same in one block (it is better to have the same number of spaces in whole code - popular option is to use 2-4 spaces, for example, this book uses 4 spaces).

Another feature of code above is empty lines. It makes reading code easier. Other syntax features will be shown during process of familiarization with data structures in Python.

.. note::
    Python has a special document that describes how best to write Python code `PEP 8 <https://pep8.org/>`__ - Style Guide for Python Code.


Comments
^^^^^^^^^^^

When writing code you often need to leave a comment, for example, to describe features of code.

Comments in Python can be one-line:

.. code:: python

    # A very important comment
    a = 10
    b = 5 # A much needed comment   

One-line comments start with pound sign. Note that comment can be in line where code itself is or in a separate line.

If it is necessary to write several lines with comments in order to not put pound sign before each line, you can make a multi-line comment:

.. code:: python

    """
    Very important
    and long comment
    """
    a = 10
    b = 5

Three double or three single quotes may be used for a multi-line comment. Comments can be used both to comment on what happens in code and to exclude execution of a particular line or block of code (i.e., to comment it).
