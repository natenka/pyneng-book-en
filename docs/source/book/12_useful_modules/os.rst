Os
---------

The ``os`` module allows working with the filesystem, environment and managing processes.

This subsection addresses only several useful features. For a more complete description of the capabilities of the module please refer to 
`documentation <https://docs.python.org/3/library/os.html>`__ or 
`article on Pymotw <https://pymotw.com/3/os/>`__.

Module **os** allows you to create directories:

.. code:: python

    In [1]: import os

    In [2]: os.mkdir('test')

    In [3]: ls -ls
    total 0
    0 drwxr-xr-x  2 nata  nata  68 Jan 23 18:58 test/

In addition, the module contains relevant existence checks. For example, if you try to re-create a directory, an error will occur:

.. code:: python

    In [4]: os.mkdir('test')
    ---------------------------------------------------------------------------
    FileExistsError                           Traceback (most recent call last)
    <ipython-input-4-cbf3b897c095> in <module>()
    ----> 1 os.mkdir('test')

    FileExistsError: [Errno 17] File exists: 'test'

In this case, testing with ``os.path.exists`` is useful:

.. code:: python

    In [5]: os.path.exists('test')
    Out[5]: True

    In [6]: if not os.path.exists('test'):
       ...:     os.mkdir('test')
       ...:

Method listdir() allows you to view the content of directory:

.. code:: python

    In [7]: os.listdir('.')
    Out[7]: ['cover3.png', 'dir2', 'dir3', 'README.txt', 'test']

By checking ``os.path.isdir`` and ``os.path.isfile`` you can get a separate list of files and list of directories:

.. code:: python

    In [8]: dirs = [ d for d in os.listdir('.') if os.path.isdir(d)]

    In [9]: dirs
    Out[9]: ['dir2', 'dir3', 'test']

    In [10]: files = [ f for f in os.listdir('.') if os.path.isfile(f)]

    In [11]: files
    Out[11]: ['cover3.png', 'README.txt']

Also in the module there are separate methods for working with paths:

.. code:: python

    In [12]: file = 'Programming/PyNEng/book/25_additional_info/README.md'

    In [13]: os.path.basename(file)
    Out[13]: 'README.md'

    In [14]: os.path.dirname(file)
    Out[14]: 'Programming/PyNEng/book/25_additional_info'

    In [15]: os.path.split(file)
    Out[15]: ('Programming/PyNEng/book/25_additional_info', 'README.md')

