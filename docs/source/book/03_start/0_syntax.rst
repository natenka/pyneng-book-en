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
    This code is shown for syntax demonstration. Although the if/else construction has not yet been considered, it is likely that the meaning of the code will be understood.

Python understands which lines refer to "if" on the indentation basis.
The execution of a block ``if a > b`` ends when another string with the same indent as the string ``if a > b`` appears. Similarly to the block “else”. 
The second feature of Python is that some expressions must be followed by colon (for example, after ``if a > b`` and after ``else``).

Several rules and recommendations on indentation:

-  Tabs or spaces can be used as indents (it is better to use spaces or more precisely to configure the editor so that the tab is 4 spaces - then when using the tab key, 4 spaces will be placed instead of 1 tab sign).
-  The number of spaces must be the same in one block (it is better to have the same number of spaces in the whole code - the popular option is to use 2-4 spaces, for example, this book uses 4 spaces).

Another feature of the code above is the empty lines. It makes reading code easier. Other syntax features will be shown during the process of familiarization with data structures in Python.

.. note::
    Python has a special document that describes how best to write Python code `PEP 8 <https://pep8.org/>`__ - the Style Guide for Python Code.


Comments
^^^^^^^^^^^

When writing code you often need to leave a comment, for example, to describe the features of the code.

Comments in Python can be one-line:

.. code:: python

    # A very important comment
    a = 10
    b = 5 # A much needed comment   

One-line comments start with the pound sign. Note that the comment can be in the line where the code itself is or in a separate line.

If it is necessary to write several lines with comments in order to not put pound sign before each line, you can make a multi-line comment:

.. code:: python

    """
    Very important
    and long comment
    """
    a = 10
    b = 5

Three double or three single quotes may be used for a multi-line comment. Comments can be used both to comment on what happens in the code and to exclude the execution of a particular line or block of code (i.e., to comment it).
