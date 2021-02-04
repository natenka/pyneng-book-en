break, continue, pass
---------------------

Python has several operators that allow to change default loop behavior.

Break operator
~~~~~~~~~~~~~~

Operator ``break`` allows early termination of loop:

* ``break`` breaks current loop and continues executing the next expressions
* if multiple nested loops are used, ``break`` interrupts internal loop and continues to execute expressions following the block. ``Break`` can be used in loops ``for`` and ``while``

Example of loop ``for``:

.. code:: python

    In [1]: for num in range(10):
       ...:     if num < 7:
       ...:         print(num)
       ...:     else:
       ...:         break
       ...:
    0
    1
    2
    3
    4
    5
    6

Example of a loop ``while``:

.. code:: python

    In [2]: i = 0
    In [3]: while i < 10:
       ...:     if i == 5:
       ...:         break
       ...:     else:
       ...:         print(i)
       ...:         i += 1
       ...:
    0
    1
    2
    3
    4

Using break in the password request example (check_password_with_while_break.py file):

.. code:: python

    username = input('Enter username: ')
    password = input('Enter password: ')

    while True:
        if len(password) < 8:
            print('Password is too short\n')
        elif username in password:
            print('Password contains username\n')
        else:
            print('Password for user {} is set'.format(username))
            # exit while loop
            break
        password = input('Enter password once again: ')

Now it is possible not to repeat string ``password = input('Enter password once again: ')`` in each branch, it is enough to move it to the end of loop.

And as soon as  correct password is entered, ``break`` will take the program out of loop ``while``.

Continue operator
~~~~~~~~~~~~~~~~~

Operator ``continue`` returns control to the beginning of loop. That is, ``continue`` allows to «jump» remaining expressions in loop and go to the next iteration.

Example of a loop ``for``:

.. code:: python

    In [4]: for num in range(5):
       ...:     if num == 3:
       ...:         continue
       ...:     else:
       ...:         print(num)
       ...:
    0
    1
    2
    4

Example of a loop ``while``:

.. code:: python

    In [5]: i = 0
    In [6]: while i < 6:
       ....:     i += 1
       ....:     if i == 3:
       ....:         print("Skip 3")
       ....:         continue
       ....:         print("No one will see it")
       ....:     else:
       ....:         print("Current value: ", i)
       ....:
    Current value:  1
    Current value:  2
    Skip 3
    Current value:  4
    Current value:  5
    Current value:  6

Use of ``continue`` in example with password request (check_password_with_while_continue.py file):

.. code:: python

    username = input('Enter username: ')
    password = input('Enter password: ')

    password_correct = False

    while not password_correct:
        if len(password) < 8:
            print('Password is too short\n')
        elif username in password:
            print('Password contains username\n')
        else:
            print('Password for user {} is set'.format(username))
            password_correct = True
            continue
        password = input('Enter password once again: ')

Here you can exit loop by checking password_correct flag. When correct password is entered, flag is set to True and with ``continue`` a jump to the beginning of loop is occurred by skipping the last line with password request.

The result will be:

::

    $ python check_password_with_while_continue.py
    Enter username: nata
    Enter password: nata12
    Password is too short

    Enter password once again: natalksdjflsdjf
    Password contains username

    Enter password once again: asdfsujljhdflaskjdfh
    Password for user nata is set

Pass operator
~~~~~~~~~~~~~

Operator ``pass`` does nothing. Basically it is a placeholder.

For example, ``pass`` can help when you need to specify a script structure. It can be set in loops, functions, classes. And it won't affect execution of code.

Example of using pass:

.. code:: python

    In [6]: for num in range(5):
       ....:     if num < 3:
       ....:         pass
       ....:     else:
       ....:         print(num)
       ....:
    3
    4

