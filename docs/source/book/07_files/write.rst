File writing
-------------

When writing information to a file, it is very important to specify the correct mode for opening the file, so as not to accidentally delete it:

*  ``w`` - open file for writing. If file exists, its content is removed
*  ``a`` - open file to add data. Data is appended to the end of the file

Both modes create a file if it does not exist.

The following methods are used to write to a file:

*  ``write()`` - write one line to file
*  ``writelines()`` - allows to send as argument a list of strings

``write()``
^^^^^^^^^^^

Method ``write`` expects string to write.

For example, take a list of lines with configuration:

.. code:: python

    In [1]: cfg_lines = ['!',
       ...:  'service timestamps debug datetime msec localtime show-timezone year',
       ...:  'service timestamps log datetime msec localtime show-timezone year',
       ...:  'service password-encryption',
       ...:  'service sequence-numbers',
       ...:  '!',
       ...:  'no ip domain lookup',
       ...:  '!',
       ...:  'ip ssh version 2',
       ...:  '!']

Open r2.txt file in write mode:

.. code:: python

    In [2]: f = open('r2.txt', 'w')

Convert the list of commands to one large string using ``join``:

.. code:: python

    In [3]: cfg_lines_as_string = '\n'.join(cfg_lines)

    In [4]: cfg_lines_as_string
    Out[4]: '!\nservice timestamps debug datetime msec localtime show-timezone year\nservice timestamps log datetime msec localtime show-timezone year\nservice password-encryption\nservice sequence-numbers\n!\nno ip domain lookup\n!\nip ssh version 2\n!'

Write a string to a file:

.. code:: python

    In [5]: f.write(cfg_lines_as_string)

Similarly, you can add a string manually:

.. code:: python

    In [6]: f.write('\nhostname r2')

After work with file is finished, it should be closed:

.. code:: python

    In [7]: f.close()

Since ipython supports *cat* command, you can easily see the content of file:

.. code:: python

    In [8]: cat r2.txt
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
    hostname r2

``writelines()``
^^^^^^^^^^^^^^^^

Method ``writelines()`` expects list of strings as an argument.

Writing cfg_lines list into the file:

.. code:: python

    In [1]: cfg_lines = ['!',
       ...:  'service timestamps debug datetime msec localtime show-timezone year',
       ...:  'service timestamps log datetime msec localtime show-timezone year',
       ...:  'service password-encryption',
       ...:  'service sequence-numbers',
       ...:  '!',
       ...:  'no ip domain lookup',
       ...:  '!',
       ...:  'ip ssh version 2',
       ...:  '!']

    In [9]: f = open('r2.txt', 'w')

    In [10]: f.writelines(cfg_lines)

    In [11]: f.close()

    In [12]: cat r2.txt
    !service timestamps debug datetime msec localtime show-timezone yearservice timestamps log datetime msec localtime show-timezone yearservice password-encryptionservice sequence-numbers!no ip domain lookup!ip ssh version 2!

As a result, all lines in the list were written into one line because there was no symbol ``\n`` at the end of lines.
You can add newline character in different ways. For example, you can loop through a list:

.. code:: python

    In [13]: cfg_lines2 = []

    In [14]: for line in cfg_lines:
       ....:     cfg_lines2.append(line + '\n')
       ....:

    In [15]: cfg_lines2
    Out[15]:
    ['!\n',
     'service timestamps debug datetime msec localtime show-timezone year\n',
     'service timestamps log datetime msec localtime show-timezone year\n',
     'service password-encryption\n',
     'service sequence-numbers\n',
     '!\n',
     'no ip domain lookup\n',
     '!\n',
     'ip ssh version 2\n',

If the final list is written anew to the file, then it will already contain newlines:

.. code:: python

    In [18]: f = open('r2.txt', 'w')

    In [19]: f.writelines(cfg_lines2)

    In [20]: f.close()

    In [21]: cat r2.txt
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

