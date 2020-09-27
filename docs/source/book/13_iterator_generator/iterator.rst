Iterators
---------

Iterator is an object that returns its elements one at a time.

From Python point of view, it is any object that has ``__next__`` method. . This method returns the next item if any, or returns the *StopIteration* exception when the items are finished.

In addition, iterator remembers which object it stopped at in the last iteration.

In Python, each iterator has ``__iter__`` - method - that is, every iterator is an iterable object. This method simply returns the iterator itself.

An example of creating an iterator from the list:

.. code:: python

    In [3]: numbers = [1, 2, 3]

    In [4]: i = iter(numbers)

Now you can use the next() function that calls ``__next__`` method to take the next element:

.. code:: python

    In [5]: next(i)
    Out[5]: 1

    In [6]: next(i)
    Out[6]: 2

    In [7]: next(i)
    Out[7]: 3

    In [8]: next(i)
    ------------------------------------------------------------
    StopIteration              Traceback (most recent call last)
    <ipython-input-8-bed2471d02c1> in <module>()
    ----> 1 next(i)

    StopIteration:

After the elements are finished, *StopIteration* exception is returned.

To make iterator to return elements again, it has to be re-created.

Similar actions are performed when loop **for** processes the list:

.. code:: python

    In [9]: for item in numbers:
       ...:     print(item)
       ...:
    1
    2
    3

When we search list elements, the iter() function is first applied to the list to create an iterator, and then ``__next__`` method is called until the *StopIteration* exception occurs.

Iterators are useful because they give elements one at a time. For example, when working with a file, it is useful that the memory will not contain the whole file, but only one line of a file.

File as iterator
~~~~~~~~~~~~~~~~~

One of the most common examples of an iterator is a file.

File r1.txt:

::

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

If we open the file with the normal open() function, we get the object that represents the file:

.. code:: python

    In [10]: f = open('r1.txt')

This object is an iterator that can be verified by calling ``__next__`` method:

.. code:: python

    In [11]: f.__next__()
    Out[11]: '!\n'

    In [12]: f.__next__()
    Out[12]: 'service timestamps debug datetime msec localtime show-timezone year\n'

You can also go through the lines using **for** loop:

.. code:: python

    In [13]: for line in f:
        ...:     print(line.rstrip())
        ...:
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    !
    no ip domain lookup
    !
    ip ssh version 2
    !

When working with files, using a file as an iterator does not simply allow iterate the file line by line - only one line is loaded into each iteration. This is very important when working with large files of thousands and hundreds of thousands of lines, such as log files.

Therefore, when working with files in Python, the most commonly used construction is:

.. code:: python

    In [14]: with open('r1.txt') as f:
        ...:     for line in f:
        ...:         print(line.rstrip())
        ...:
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

