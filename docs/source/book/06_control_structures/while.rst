while
-----

A **while** loop is another type of loop in Python.

Unlike **if**, after executing code in block, **while** returns to the beginning of loop.

When using **while** loops it is necessary to pay attention to whether the result when condition of loop is false will be reached.

Consider a simple example:

.. code-block:: python

    In [1]: a = 5

    In [2]: while a > 0:
       ...:     print(a)
       ...:     a -= 1 # This record is equal to: a = a - 1
       ...:
    5
    4
    3
    2
    1

First, create a variable with a value of 5.

Then, in **while** loop the condition a > 0 is specified. That is, as long as value is greater than 0, actions in the body of loop will be performed. In this case, value of variable *a* will be displayed.

In addition, in the body of loop after each pass, *a* value becomes one less.

.. note::
    Record ``a -= 1`` can be a bit unusual. Python allows this format to be used instead of ``a = a - 1``.

    Similarly, you can write: ``a += 1``, ``a *= 2``,
    ``a /= 2``.

As *a* value decreases, loop will not be infinite and at some point the expression a > 0 becomes false.

The following example is based on example about password from section which describes **if** construction use :ref:`if_example`.
In that example you had to restart script if password did not meet requirements.

With **while** loop you can make sure that  script itself requests password again if it does not meet requirements.

Check_password_with_while.py file:

.. code:: python

    # -*- coding: utf-8 -*-

    username = input('Enter username: ' )
    password = input('Enter password: ' )

    password_correct = False

    while not password_correct:
        if len(password) < 8:
            print('Password is too short\n')
            password = input('Enter password once again: ' )
        elif username in password:
            print('Password contains username\n')
            password = input('Enter password once again: ' )
        else:
            print('Password for user {} is set'.format( username ))
            password_correct = True

In this case, **while** loop is useful because it returns script back to the beginning of checks and allows password to be typed again but does not require script to restart.

Now script works like this:

::

    $ python check_password_with_while.py
    Enter username: nata
    Enter password: nata
    Password is too short

    Enter password once again: natanata
    Password contains username

    Enter password once again: 123345345345
    Password for user nata is set

