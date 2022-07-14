os
===

The os module allows you to work with the file system, with the environment,
and manage processes.

This subsection covers only a few useful features. For a more complete
description of the module's capabilities, see the `documentation
<https://docs.python.org/3/library/os.html>`__ or `article on pymotw
<https://pymotw.com/3/os/>`__.


Module import:

.. code:: python

    In [1]: import os

os.environ
----------

os.environ returns a dictionary with environment variables and their values.
You can use square brackets to get variable if the environment
variable definitely exists (an exception will be raised if the variable does
not exist).

.. code:: python

    In [2]: os.environ["HOME"]
    Out[2]: '/home/nata'

    In [3]: os.environ["TOKEN"]
    ---------------------------------------------------------------------------
    KeyError                                  Traceback (most recent call last)
    Input In [3], in <cell line: 1>()
    ----> 1 os.environ["TOKEN"]

    File ~/venv/pyneng-py3-8-0/lib/python3.8/os.py:673, in _Environ.__getitem__(self, key)
        670     value = self._data[self.encodekey(key)]
        671 except KeyError:
        672     # raise KeyError with the original key value
    --> 673     raise KeyError(key) from None
        674 return self.decodevalue(value)

    KeyError: 'TOKEN'

Or use the get method, then if there is no environment variable, None is returned:

.. code:: python

    In [3]: os.environ.get("HOME")
    Out[3]: '/home/nata'

    In [4]: os.environ.get("TOKEN")


.. note::

    Technically, os.environ returns an object of type `mapping
    <https://docs.python.org/3/glossary.html#term-mapping>`__, but at this
    point it's easier to think of it as a dictionary.


Environment variables are read the first time the os module is imported.
If some variables were added while the script was running, they will not be
available through ``os.environ``.

os.mkdir
--------

os.mkdir allows you to create a directory:

.. code:: python

    In [2]: os.mkdir('test')

    In [3]: ls -ls
    total 0
    0 drwxr-xr-x  2 nata  nata  68 Jan 23 18:58 test/

os.listdir
----------

The listdir function returns a list of files and subdirectories in the
specified directory. The order of the files in the list is arbitrary, if you
need to sort them, you can use the sorted function.

.. code:: python

    In [2]: os.listdir("09_functions")
    Out[2]:
    ['test_task_9_4.py',
     'task_9_2a.py',
     'task_9_1a.py',
     'test_task_9_2.py',
     'task_9_3a.py',
     'test_task_9_3a.py',
     'task_9_3.py',
     'test_task_9_3.py',
     'config_sw2.txt',
     'test_task_9_2a.py',
     'config_sw1.txt',
     'test_task_9_1a.py',
     'test_task_9_1.py',
     'task_9_4.py',
     'task_9_1.py',
     'config_r1.txt',
     'task_9_2.py']

    In [3]: sorted(os.listdir("09_functions"))
    Out[3]:
    ['config_r1.txt',
     'config_sw1.txt',
     'config_sw2.txt',
     'task_9_1.py',
     'task_9_1a.py',
     'task_9_2.py',
     'task_9_2a.py',
     'task_9_3.py',
     'task_9_3a.py',
     'task_9_4.py',
     'test_task_9_1.py',
     'test_task_9_1a.py',
     'test_task_9_2.py',
     'test_task_9_2a.py',
     'test_task_9_3.py',
     'test_task_9_3a.py',
     'test_task_9_4.py']

The current directory can be specified as ``"."`` or call listdir with no arguments:

.. code:: python

    In [7]: os.listdir('.')
    Out[7]: ['cover3.png', 'dir2', 'dir3', 'README.txt', 'test']

    In [7]: os.listdir()
    Out[7]: ['cover3.png', 'dir2', 'dir3', 'README.txt', 'test']

os.path
-------

Different operating systems (OS) have different path naming conventions, so
there are multiple versions of the os.path module in the standard library. The
os module automatically loads the necessary part to work with the current OS.
For example, when running the same functions of the os module on Windows and
Linux, different values will be considered as the path separator.

If you need to work on Linux with Windows paths and vice versa, you can use the
``posixpath``, ``ntpath`` modules instead of ``os.path``.


os.path.exists
~~~~~~~~~~~~~~

The ``os.path.exists`` function checks if the specified path exists
and returns True if it exists and False otherwise:

.. code:: python

    In [5]: os.path.exists('test')
    Out[5]: True

    In [6]: if not os.path.exists('test'):
       ...:     os.mkdir('test')
       ...:

os.path.isdir, os.path.isfile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``os.path.isdir`` function returns True if the path leads to a directory, False otherwise:

.. code:: python

    In [4]: os.path.isdir("09_functions")
    Out[4]: True

    In [5]: os.path.isdir("/home/nata/repos/pyneng-tasks/exercises/09_functions/")
    Out[5]: True

    In [6]: os.path.isdir("/home/nata/repos/pyneng-tasks/exercises/09_functions/task_9_1.py")
    Out[6]: False

    In [7]: os.path.isdir("09_functions/task_9_1.py")
    Out[7]: False

The ``os.path.isfile`` function returns True if the path leads to a file and False otherwise:

.. code:: python

    In [9]: os.path.isfile("09_functions/task_9_1.py")
    Out[9]: True

    In [10]: os.path.isfile("09_functions/")
    Out[10]: False


Using the checks ``os.path.isdir``, ``os.path.isfile`` and ``os.listdir``, you
can get lists of files and directories (in the example for the current
directory).

List of directories in the current directory:

.. code:: python

    In [8]: dirs = [d for d in os.listdir('.') if os.path.isdir(d)]

    In [9]: dirs
    Out[9]: ['dir2', 'dir3', 'test']

List of files in the current directory:

.. code:: python

    In [10]: files = [f for f in os.listdir('.') if os.path.isfile(f)]

    In [11]: files
    Out[11]: ['cover3.png', 'README.txt']


os.path.split
~~~~~~~~~~~~~

The os.path.split function splits the path into the "main part" and the end of
the path at the last ``/`` and returns a tuple of two elements. This will
automatically use a backslash for Windows.

If there is no slash at the end of the path, the division will be like this

.. code:: python

    In [6]: os.path.split("book/25_additional_info/README.md")
    Out[6]: ('book/25_additional_info', 'README.md')

    In [8]: os.path.split("book/25_additional_info")
    Out[8]: ('book', '25_additional_info')

If the path ends with a slash, the second element of the tuple will be an empty string:

.. code:: python

    In [7]: os.path.split("book/25_additional_info/")
    Out[7]: ('book/25_additional_info', '')

    In [9]: os.path.split("book/")
    Out[9]: ('book', '')

If the path ends with a slash, the second element of the tuple will be an empty string:

.. code:: python

    In [39]: os.path.split("README.md")
    Out[39]: ('', 'README.md')

os.path.abspath
~~~~~~~~~~~~~~~~

The os.path.abspath function returns the absolute path for the specified file or directory:

.. code:: python

    In [40]: os.path.abspath("09_functions")
    Out[40]: '/home/nata/repos/pyneng-tasks/exercises/09_functions'

    In [41]: os.path.abspath(".")
    Out[41]: '/home/nata/repos/pyneng-tasks/exercises'

