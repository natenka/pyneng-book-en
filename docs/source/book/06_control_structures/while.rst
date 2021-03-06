while
-----

A ``while`` loop is another type of loop in Python.

In the while loop, as in the if statement, you need to write a condition. 
If the condition is true, the actions inside the while block are executed. 
In this case, unlike if, after executing the code in the block, while returns to the beginning of the loop.

When using ``while`` loops it is necessary to pay attention to whether the result when condition of loop is false will be reached.

Consider an example:

.. code:: python

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

First, variable A is created with the value 5.

Then, in ``while`` loop the condition ``a > 0`` is specified. 
That is, as long as the value of ``a`` is greater than 0, actions in the body of the loop will be executed. 
In this case, value of variable ``a`` will be displayed.

In addition, in the body of the loop, with each pass, the value of ``a`` is decreased by one.

.. note::

    Record ``a -= 1`` can be a bit unusual. Python allows this format to be used instead of ``a = a - 1``.

    Similarly, you can write: ``a += 1``, ``a *= 2``,
    ``a /= 2``.

Since the value of ``a`` is decreasing, the loop will not be infinite, and at some point 
the expression ``a > 0`` will become false.

The following example is based on example about password from section which
describes ``if`` statement use :ref:`if_example`.
In that example, you had to re-run the script if the password did not meet the
requirements. Using a while loop, you can make the script ask for a password again
if it does not meet the requirements (check_password_with_while.py):

.. code:: python

    # -*- coding: utf-8 -*-

    username = input('Enter username: ')
    password = input('Enter password: ')

    password_correct = False

    while not password_correct:
        if len(password) < 8:
            print('Password is too short\n')
            password = input('Enter password once again: ')
        elif username in password:
            print('Password contains username\n')
            password = input('Enter password once again: ' )
        else:
            print('Password for user {} is set'.format(username))
            password_correct = True

In this case, ``while`` loop is useful because it returns script back to the beginning of checks and allows password to be typed again but does not require script to restart.

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

