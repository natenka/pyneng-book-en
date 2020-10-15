File reading
-------------

Python has several file reading methods:

* ``read()`` - reads the contents of file to string
* ``readline()`` - reads file line by line
* ``readlines()`` - reads file lines and creates a list from the lines

Let’s see how to read contents of files using the example of r1.txt:

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

``read()``
^^^^^^^^^^

Method ``read()`` reads the entire file to one string.

Example of the use of ``read()``:

.. code:: python

    In [1]: f = open('r1.txt')

    In [2]: f.read()
    Out[2]: '!\nservice timestamps debug datetime msec localtime show-timezone year\nservice timestamps log datetime msec localtime show-timezone year\nservice password-encryption\nservice sequence-numbers\n!\nno ip domain lookup\n!\nip ssh version 2\n!\n'

    In [3]: f.read()
    Out[3]: ''

When reading a file once again an empty line is displayed in line 3. This is because the whole file is read when ``read()`` method is called. And after the file has been read the cursor stays at the end of file. The cursor position can be controlled by means of ``seek()`` method.

``readline()``
^^^^^^^^^^^^^^

File can be read line by line using ``readline()`` method:

.. code:: python

    In [4]: f = open('r1.txt')

    In [5]: f.readline()
    Out[5]: '!\n'

    In [6]: f.readline()
    Out[6]: 'service timestamps debug datetime msec localtime show-timezone year\n'

But most often it is easier to walk through a **file** object in a loop without using  ``read...`` methods:

.. code:: python

    In [7]: f = open('r1.txt')

    In [8]: for line in f:
       ...:     print(line)
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

``readlines()``
^^^^^^^^^^^^^^^

Another useful method is ``readlines()``. It reads file lines to the list:

.. code:: python

    In [9]: f = open('r1.txt')

    In [10]: f.readlines()
    Out[10]:
    ['!\n',
     'service timestamps debug datetime msec localtime show-timezone year\n',
     'service timestamps log datetime msec localtime show-timezone year\n',
     'service password-encryption\n',
     'service sequence-numbers\n',
     '!\n',
     'no ip domain lookup\n',
     '!\n',
     'ip ssh version 2\n',
     '!\n']

If you want to get lines of a file but without a line feed character at the end, you can use ``split()`` method and specify symbol ``\n`` as a separator:

::

    In [11]: f = open('r1.txt')

    In [12]: f.read().split('\n')
    Out[12]:
    ['!',
     'service timestamps debug datetime msec localtime show-timezone year',
     'service timestamps log datetime msec localtime show-timezone year',
     'service password-encryption',
     'service sequence-numbers',
     '!',
     'no ip domain lookup',
     '!',
     'ip ssh version 2',
     '!',
     '']

Note that the last item in list is an empty string.

If you use ``split()`` before ``rstrip()``, list will be without empty string at the end:

.. code:: python

    In [13]: f = open('r1.txt')

    In [14]: f.read().rstrip().split('\n')
    Out[14]:
    ['!',
     'service timestamps debug datetime msec localtime show-timezone year',
     'service timestamps log datetime msec localtime show-timezone year',
     'service password-encryption',
     'service sequence-numbers',
     '!',
     'no ip domain lookup',
     '!',
     'ip ssh version 2',
     '!']

``seek()``
^^^^^^^^^^

Until now, file had to be reopened to read it again. This is because after reading methods a cursor is at the end of the file. And second reading returns an empty string.

To read information from a file again you need to use the 
``seek`` method which moves the cursor to the desired position.

Example of file opening and content reading:

.. code:: python

    In [15]: f = open('r1.txt')

    In [16]: print(f.read())
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

If you call ``read`` method again an empty string returns:

.. code:: python

    In [17]: print(f.read())

But with ``seek`` method you can go to the beginning of file (0 means the beginning of file):

.. code:: python

    In [18]: f.seek(0)

Once cursor has been set to the beginning of file you can read the content again:

.. code:: python

    In [19]: print(f.read())
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

