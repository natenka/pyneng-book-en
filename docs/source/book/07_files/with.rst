with statement
----------------

The ``with`` statement is called the context manager.

Python has a more convenient way of working with files than the ones used so far - construction ``with``:

.. code:: python

    In [1]: with open('r1.txt', 'r') as f:
      ....:     for line in f:
      ....:         print(line)
      ....:
    !

    service timestamps debug datetime msec localtime show-timezone year

    service timestamps log datetime msec localtime show-timezone year

    service password-encryption

    service sequence-numbers

    !

    no ip domain lookup

    !

    ip ssh version 2

    !

In addition, construction ``with`` guarantees file closure automatically.

Pay attention to how lines of the file are read:

.. code:: python

    for line in f:
        print(line)

When file needs to be run line by line, it is best to use this option.

In previous output there were extra empty lines between lines of the file because ``print`` adds another line feed character.

To get rid of this you can use ``rstrip`` method:

.. code:: python

    In [2]: with open('r1.txt', 'r') as f:
      ....:     for line in f:
      ....:         print(line.rstrip())
      ....:
    !
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    !
    no ip domain lookup
    !
    ip ssh version 2
    !

    In [3]: f.closed
    Out[3]: True

And of course, ``with`` construction can be used not only as a line-by-line reader, all methods that have been considered before also work:

.. code:: python

    In [4]: with open('r1.txt', 'r') as f:
      ....:     print(f.read())
      ....:
    !
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    !
    no ip domain lookup
    !
    ip ssh version 2
    !

Open two files
~~~~~~~~~~~~~~~~~~~~

Sometimes you have to work with two files simultaneously. For example, write some lines from one file to another.

In this case you can open two files in ``with`` block as follows:

.. code:: python

    In [5]: with open('r1.txt') as src, open('result.txt', 'w') as dest:
       ...:     for line in src:
       ...:         if line.startswith('service'):
       ...:             dest.write(line)
       ...:

    In [6]: cat result.txt
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers

This is equivalent to:

.. code:: python

    In [7]: with open('r1.txt') as src:
       ...:     with open('result.txt', 'w') as dest:
       ...:         for line in src:
       ...:             if line.startswith('service'):
       ...:                 dest.write(line)
       ...:

