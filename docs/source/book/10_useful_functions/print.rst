.. _print:

print
-----

Function ``print`` has been used many times in book, but its full
syntax has not yet been discussed:

.. code:: python

    print(*items, sep=' ', end='\n', file=sys.stdout, flush=False)

Function ``print`` outputs all elements by separating them by their **sep** value and finishes output with **end** value.

All elements that are passed as arguments are converted into strings:

.. code:: python

    In [4]: def f(a):
       ...:     return a
       ...:

    In [5]: print(1, 2, f, range(10))
    1 2 <function f at 0xb4de926c> range(0, 10)

For functions f and range the result is equivalent to str:

.. code:: python

    In [6]: str(f)
    Out[6]: '<function f at 0xb4de926c>'

    In [7]: str(range(10))
    Out[7]: 'range(0, 10)'

sep
~~~

Parameter **sep** controls which separator will be used between elements.

By default, space is used:

.. code:: python

    In [8]: print(1, 2, 3)
    1 2 3

You can change **sep** value to any other string:

.. code:: python

    In [9]: print(1, 2, 3, sep='|')
    1|2|3

    In [10]: print(1, 2, 3, sep='\n')
    1
    2
    3

    In [11]: print(1, 2, 3, sep='\n'+'-'*10+'\n')
    1
    ----------
    2
    ----------
    3

.. note::
    Note that all arguments that manage behavior of ``print`` function must be passed on as keyword, not positional.

In some situations ``print`` function can replace join method:

.. code:: python

    In [12]: items = [1, 2, 3, 4, 5]

    In [13]: print(*items, sep=', ')
    1, 2, 3, 4, 5

end
~~~

Parameter **end** controls which value will be displayed after all elements are printed. 
By default, line feed character is used:

.. code:: python

    In [19]: print(1, 2, 3)
    1 2 3

You can change **end** value to any other string:

.. code:: python

    In [20]: print(1, 2, 3, end='\n'+'-'*10)
    1 2 3
    ----------

file
~~~~

Parameter **file** controls where values of ``print`` function are displayed. The default output is sys.stdout.

Python allows to pass to **file** as an argument any object with write(string) method. 

.. code:: python

    In [1]: f = open('result.txt', 'w')

    In [2]: for num in range(10):
       ...:     print('Item {}'.format(num), file=f)
       ...:

    In [3]: f.close()

    In [4]: cat result.txt
    Item 0
    Item 1
    Item 2
    Item 3
    Item 4
    Item 5
    Item 6
    Item 7
    Item 8
    Item 9

flush
~~~~~

By default, when writing to a file or print to a standard output stream, the output is buffered.
Function ``print`` allows to disable buffering. You can control it in a file.

Example script that displays a number from 0 to 10 every second (print_nums.py file):

.. code:: python

    import time

    for num in range(10):
        print(num)
        time.sleep(1)

Try running a script and make sure the numbers are displayed once per second.

Now, a similar script but the numbers will appear in one line (print_nums_oneline.py file):

.. code:: python

    import time

    for num in range(10):
        print(num, end=' ')
        time.sleep(1)

Try running a function. Numbers does not appear one per second but all appear after 10 seconds.

This is because when output is displayed on standard output, **flush** is performed after line feed character.

In order to make script work properly **flush** should be set to True (print_nums_oneline_fixed.py file):

.. code:: python

    import time

    for num in range(10):
        print(num, end=' ', flush=True)
        time.sleep(1)

