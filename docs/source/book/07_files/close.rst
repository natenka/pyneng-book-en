File closing
---------------

.. note::
    In real life, the most common way to close files is use of ``with``
    construction. It’s much more convenient way than to close file explicitly.
    But since you can also find ``close`` method in life, this section discusses how to use it.
    
After you finish working with file you have to close it. In some cases Python can close file itself. But it’s best not to count on it and close file explicitly.

``close``
^^^^^^^^^^^

Method close() met in `File writing  <https://pyneng.readthedocs.io/en/latest/book/07_files/3_write.html>`__ section.
It was there to make sure that the content of file was written on disk.

For this, Python has a separate ``flush`` method.
But since in example with file writing there was no need to perform any more operations, file could be closed.

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
The **file** object has a special ``closed`` attribute that lets you check whether a file is closed or not. If file is open, it returns ``False``:

.. code:: python

    In [3]: f.closed
    Out[3]: False

Now close file and check ``closed`` again:

.. code:: python

    In [4]: f.close()

    In [5]: f.closed
    Out[5]: True

If you try to read file an exception occurs:

.. code:: python

    In [6]: print(f.read())
    ------------------------------------------------------------------
    ValueError                       Traceback (most recent call last)
    <ipython-input-53-2c962247edc5> in <module>()
    ----> 1 print(f.read())

    ValueError: I/O operation on closed file

