Python interpreter. IPython
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Interpreter makes it possible to receive an instant response to executed actions.
You can say that interpreter works as CLI (Command Line Interface) of network
devices: each command will be executed immediately after pressing Enter.
However, there is an exception: more complex objects (such as cycles or
functions) are executed only after twice pressing Enter.

In previous section, a standard interpreter was called to verify Python
installation. There is also an improved interpreter
`IPython <http://ipython.readthedocs.io/en/stable/index.html>`__.
IPython allows much more than standard interpreter called by "python" command.
Some examples (IPython features are much broader):

-  Autocomplete Tab commands or hints if there are more than one command version
-  More structured and understandable output of commands
-  Automatic indentation in loops and other objects
-  You can either walk through the command execution history or watch it with %history 'magic' command

You can install IPython using pip (installation will be done in a virtual environment if configured):

::

    pip install ipython

After that, you can move to IPython as follows:

::

    $ ipython
    Python 3.7.3 (default, May 13 2019, 15:44:23)
    Type 'copyright', 'credits' or 'license' for more information
    IPython 7.5.0 -- An enhanced Interactive Python. Type '?' for help.

    In [1]:

Command ``quit`` is used to exit. The following is how IPython will be used.

To get acquainted with interpreter, you can use it as a calculator:

.. code:: python

    In [1]: 1 + 2
    Out[1]: 3

    In [2]: 22*45
    Out[2]: 990

    In [3]: 2**3
    Out[3]: 8

In IPython, input and output are marked:

-  In - user input data
-  Out - result that command returns (if any)
-  Numbers after In or Out are sequential numbers of executed commands in current IPython session

Example of string output by function ``print``:

.. code:: python

    In [4]: print('Hello!')
    Hello!

When a loop is created in interpreter, for example, invitation changes to ellipsis inside loop. To complete loop and exit this shortcut, double press Enter:

.. code:: python

    In [5]: for i in range(5):
       ...:     print(i)
       ...:
    0
    1
    2
    3
    4

help()
^^^^^^

In IPython you can view help for an arbitrary object, function or method using ``help``:

::

    In [1]: help(str)
    Help on class str in module builtins:

    class str(object)
     |  str(object='') -> str
     |  str(bytes_or_buffer[, encoding[, errors]]) -> str
     |
     |  Create a new string object from given object. If encoding or
     |  errors is specified, then object must expose a data buffer
     |  that will be decoded using given encoding and error handler.
    ...

    In [2]: help(str.strip)
    Help on method_descriptor:

    strip(...)
        S.strip([chars]) -> str

        Return a copy of string S with leading and trailing
        whitespace removed.
        If chars is given and not None, remove characters in chars instead.

The second option is:

::

    In [3]: ?str
    Init signature: str(self, /, *args, **kwargs)
    Docstring:
    str(object='') -> str
    str(bytes_or_buffer[, encoding[, errors]]) -> str

    Create a new string object from given object. If encoding or
    errors is specified, then object must expose a data buffer
    that will be decoded using given encoding and error handler.
    Otherwise, returns the result of object.__str__() (if defined)
    or repr(object).
    encoding defaults to sys.getdefaultencoding().
    errors defaults to 'strict'.
    Type:           type

    In [4]: ?str.strip
    Docstring:
    S.strip([chars]) -> str

    Return a copy of string S with leading and trailing
    whitespace removed.
    If chars is given and not None, remove characters in chars instead.
    Type:      method_descriptor

``print``
^^^^^^^

Function ``print`` displays information on a standard output (current
terminal screen). If you want to get a string, you should place it in
quotes(double or single). If you want to get, for example, a computation result
or just a number, quotes are not needed:

.. code:: python

    In [6]: print('Hello!')
    Hello!

    In [7]: print(5*5)
    25

If you want to get several values in a row through a space, you have to enumerate them through a comma:

.. code:: python

    In [8]: print(1*5, 2*5, 3*5, 4*5)
    5 10 15 20

    In [9]: print('one', 'two', 'three')
    one two three

By default, at the end of each expression passed to ``print``, there will be a
new line character. If it is necessary that after the output of each expression there would
be no new line, an additional "end" argument should be specified as the last expression in ``print``.

.. seealso:: Additional parameters of print function :ref:`print`

dir()
^^^^^

Function ``dir`` can be used to see what attributes (variables tied to object) and methods (functions tied to object) are available.

For example, for number the output will be (pay attention on various methods that allow arithmetic operations):

.. code:: python

    In [10]: dir(5)
    Out[10]:
    ['__abs__',
     '__add__',
     '__and__',
     ...
     'bit_length',
     'conjugate',
     'denominator',
     'imag',
     'numerator',
     'real']

The same for string:

.. code:: python

    In [11]: dir('hello')
    Out[11]:
    ['__add__',
     '__class__',
     '__contains__',
     ...
     'startswith',
     'strip',
     'swapcase',
     'title',
     'translate',
     'upper',
     'zfill']

If you call ``dir`` with no value, it shows existing methods, attributes,
and variables defined in current session of interpreter:

.. code:: python

    In [12]: dir()
    Out[12]:
    ['__builtin__',
     '__builtins__',
     '__doc__',
     '__name__',
     '_dh',
     ...
     '_oh',
     '_sh',
     'exit',
     'get_ipython',
     'i',
     'quit']

For example, after creating variable a and test():

.. code:: python

    In [13]: a = 'hello'

    In [14]: def test():
       ....:     print('test')
       ....:

    In [15]: dir()
    Out[15]:
     ...
     'a',
     'exit',
     'get_ipython',
     'i',
     'quit',
     'test']

