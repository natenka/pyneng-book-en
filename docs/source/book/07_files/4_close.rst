File closing
---------------

.. note::
    In real life, the most common way to close files is use of ``with`` construction. It’s much more convenient way than to close file explicitly. But since you can also find the ``close`` method in life, this section discusses how to use it.
    
After you finish working with file you have to close it. In some cases Python can close the file itself. But it’s best not to count on it and close the file explicitly.

``close()``
^^^^^^^^^^^

The close() method met in `File writing  <./3_write.md>`__ section.
It was there to make sure that the content of the file was written on disk.

For this, Python has a separate ``flush()`` method.
But since in the example with the file writing there was no need to perform any more operations, the file could be closed.

Open the r1.txt file:

.. code:: python

    In [1]: f = open('r1.txt', 'r')

You can now read the content:

.. code:: python

    In [2]: print(f.read())
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
The **file** object has a special ``closed`` attribute that lets you check whether a file is closed or not. If the file is open, it returns ``False``:

.. code:: python

    In [3]: f.closed
    Out[3]: False

Now close the file and check ``closed`` again:

.. code:: python

    In [4]: f.close()

    In [5]: f.closed
    Out[5]: True

If you try to read the file an exception occurs:

.. code:: python

    In [6]: print(f.read())
    ------------------------------------------------------------------
    ValueError                       Traceback (most recent call last)
    <ipython-input-53-2c962247edc5> in <module>()
    ----> 1 print(f.read())

    ValueError: I/O operation on closed file

Use ``try/finally`` to work with files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By processing exceptions, you can:

-  intercept exceptions that occur when trying to read a non-existent file
-  close file after all operations in ``finally`` block

If you try to read a file that does not exist this exception will occur:

.. code:: python

    In [7]: f = open('r3.txt', 'r')
    ---------------------------------------------------------------------------
    IOError                                   Traceback (most recent call last)
    <ipython-input-54-1a33581ca641> in <module>()
    ----> 1 f = open('r3.txt', 'r')

    IOError: [Errno 2] No such file or directory: 'r3.txt'

Using ``try/except`` construction you can capture this exception and print your message:

.. code:: python

    In [8]: try:
      ....:     f = open('r3.txt', 'r')
      ....: except IOError:
      ....:     print('No such file')
      ....:
    No such file

And with ``finally`` you can close the file after all operations:

.. code:: python

    In [9]: try:
      ....:     f = open('r1.txt', 'r')
      ....:     print(f.read())
      ....: except IOError:
      ....:     print('No such file')
      ....: finally:
      ....:     f.close()
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

    In [10]: f.closed
    Out[10]: True

