Working with try/except/else/finally
---------------------------------------------

try/except
~~~~~~~~~~

If you repeated examples that were used before, there could be situations where a mistake was made. 
It was probably a syntax error when a colon was missing, for example.
Python generally reacts quite understandably to such errors and they can easily be corrected.
However, even if the code is syntactically correct, errors can occur. In Python, these errors are called *exceptions*.

Examples of exceptions:

.. code:: python

    In [1]: 2/0
    -----------------------------------------------------
    ZeroDivisionError: division by zero

    In [2]: 'test' + 2
    -----------------------------------------------------
    TypeError: must be str, not int

In this case, two exceptions were raised: ``ZeroDivisionError`` and ``TypeError``.

Most often, it is possible to predict what kind of exceptions will occur during execution of the program.
For example, if program expects two numbers in input and output returns their sum and user has entered a string instead of one of numbers a TypeError error will appear as in example above.

Python allows working with exceptions. They can be intercepted and acted upon if an exception has been occurred.

.. note::

    When an exception appears, program is immediately interrupted.

In order to work with exceptions ``try/except`` construction is used:

.. code:: python

    In [3]: try:
       ...:     2/0
       ...: except ZeroDivisionError:
       ...:     print("You can't divide by zero")
       ...:
    You can't divide by zero

The ``try`` construction works as follows:

* first execute expressions that are written in ``try`` block
* if there are no exceptions during execution of ``try`` block, block ``except`` is skipped and the following code is executed
* if there is an exception within ``try`` block, the rest part of ``try`` block is skipped

  * if ``except`` block contains an exception which has been occurred, code in ``except`` block is executed
  * if exception that has raised is not specified in ``except`` block, program execution is interrupted and an error is generated

Note that ``Cool!`` string in ``try`` block is not displayed:

.. code:: python

    In [4]: try:
       ...:     print("Let's divide some numbers")
       ...:     2/0
       ...:     print('Cool!')
       ...: except ZeroDivisionError:
       ...:     print("You can't divide by zero")
       ...:
    Let's divide some numbers
    You can't divide by zero

Construction try/except may have many ``except`` if different actions are needed depending on type of error.

For example, divide.py script divides two numbers entered by user:

.. code:: python

    # -*- coding: utf-8 -*-

    try:
        a = input("Enter first number: ")
        b = input("Enter second number: ")
        print("Result: ", int(a)/int(b))
    except ValueError:
        print("Please enter only numbers")
    except ZeroDivisionError:
        print("You can't divide by zero")

Examples of script execution:

::

    $ python divide.py
    Enter first number: 3
    Enter second number: 1
    Result:  3

    $ python divide.py
    Enter first number: 5
    Enter second number: 0
    You can't divide by zero

    $ python divide.py
    Enter first number: qewr
    Enter second number: 3
    Please enter only numbers

In this case, ValueError exception occurs when user has entered a string instead of a number.

ZeroDivisionError exception occurs if second number is 0.

If you do not need to display different messages on ValueError
and ZeroDivisionError, you can do this (divide\_ver2.py file):

.. code:: python

    # -*- coding: utf-8 -*-

    try:
        a = input("Enter first number: ")
        b = input("Enter second number: ")
        print("Result: ", int(a)/int(b))
    except (ValueError, ZeroDivisionError):
        print("Something went wrong...")

Verification:

.. code:: python

    $ python divide_ver2.py
    Enter first number: wer
    Enter second number: 4
    Something went wrong...

    $ python divide_ver2.py
    Enter first number: 5
    Enter second number: 0
    Something went wrong...

.. note::
    In block ``except`` you don’t have to specify a specific exception or exceptions. In that case, all exceptions would be intercepted.

    ``That is not recommended!``

try/except/else
~~~~~~~~~~~~~~~

Try/except has an optional ``else`` block. It is implemented if there is no exception.

For example, if you need to perform any further operations with data that user entered, you can write them in ``else`` block (divide_ver3.py file):

.. code:: python

    # -*- coding: utf-8 -*-

    try:
        a = input("Enter first number: ")
        b = input("Enter second number: ")
        result = int(a)/int(b)
    except (ValueError, ZeroDivisionError):
        print("Something went wrong...")
    else:
        print("Result is squared: ", result``2)

Example of execution:

.. code:: python

    $ python divide_ver3.py
    Enter first number: 10
    Enter second number: 2
    Result is squared:  25

    $ python divide_ver3.py
    Enter first number: werq
    Enter second number: 3
    Something went wrong...

try/except/finally
~~~~~~~~~~~~~~~~~~

Block ``finally`` is another optional block in ``try`` construction. It is *always* implemented, whether an exception has been raised or not.

It’s about actions that you have to do anyway. For example, it could be a file closing.

File divide_ver4.py с блоком finally:

.. code:: python

    # -*- coding: utf-8 -*-

    try:
        a = input("Enter first number: ")
        b = input("Enter second number: ")
        result = int(a)/int(b)
    except (ValueError, ZeroDivisionError):
        print("Something went wrong...")
    else:
        print("Result is squared: ", result``2)
    finally:
        print("And they lived happily ever after.")

Verification:

.. code:: python

    $ python divide_ver4.py
    Enter first number: 10
    Enter second number: 2
    Result is squared:  25
    And they lived happily ever after.

    $ python divide_ver4.py
    Enter first number: qwerewr
    Enter second number: 3
    Something went wrong...
    And they lived happily ever after.

    $ python divide_ver4.py
    Enter first number: 4
    Enter second number: 0
    Something went wrong...
    And they lived happily ever after.

When to use exceptions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As a rule, same code can be written with or without exceptions.

For example, this version of code:

.. code:: python

    while True:
        a = input("Enter first number: ")
        b = input("Enter second number: ")
        try:
            result = int(a)/int(b)
        except ValueError:
            print("Only digits are supported")
        except ZeroDivisionError:
            print("You can't divide by zero")
        else:
            print(result)
            break

You can rewrite this without try/except (try_except_divide.py file):

.. code:: python

    while True:
        a = input("Enter first number: ")
        b = input("Enter second number: ")
        if a.isdigit() and b.isdigit():
            if int(b) == 0:
                print("You can't divide by zero")
            else:
                print(int(a)/int(b))
                break
        else:
            print("Only digits are supported")

But the same option without exceptions will not always be simple and understandable.

It is important to assess in each specific situation which version of code is more comprehensible, compact and universal - with or without exceptions.

If you’ve used some other programming language before, it’s possible that use of exceptions was considered as a bad form. In Python this is not true. To get a little bit more into this issue, look at the links to additional material at the end of this section.
