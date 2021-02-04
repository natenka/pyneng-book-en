Unicode in Python 3
-----------------

Python 3 has:

* strings - an immutable sequence of Unicode characters. Type string (str) is used to store these characters
* bytes - an immutable sequence of bytes. Type bytes is used for storage

Strings
~~~~~~

Examples of strings:

.. code:: python

    In [11]: hi = 'привет'

    In [12]: hi
    Out[12]: 'привет'

    In [15]: type(hi)
    Out[15]: str

    In [13]: beautiful = 'schön'

    In [14]: beautiful
    Out[14]: 'schön'

Since strings are a sequence of Unicode codes you can write a string in different ways.

Unicode symbol can be written using its name:

.. code:: python

    In [1]: "\N{LATIN SMALL LETTER O WITH DIAERESIS}"
    Out[1]: 'ö'

Or by using this format:

.. code:: python

    In [4]: "\u00F6"
    Out[4]: 'ö'

You can write a string as a sequence of Unicode codes:

.. code:: python

    In [19]: hi1 = 'привет'

    In [20]: hi2 = '\u043f\u0440\u0438\u0432\u0435\u0442'

    In [21]: hi2
    Out[21]: 'привет'

    In [22]: hi1 == hi2
    Out[22]: True

    In [23]: len(hi2)
    Out[23]: 6

Function ord() returns value of Unicode code for character:

.. code:: python

    In [6]: ord('ö')
    Out[6]: 246

Function chr() returns Unicode character that corresponds to the code:

.. code:: python

    In [7]: chr(246)
    Out[7]: 'ö'

Bytes
~~~~~

Bytes are an immutable sequence of bytes.

Bytes are denoted in the same way as strings but with addition of letter ``b``
before string:

.. code:: python

    In [30]: b1 = b'\xd0\xb4\xd0\xb0'

    In [31]: b2 = b"\xd0\xb4\xd0\xb0"

    In [32]: b3 = b'''\xd0\xb4\xd0\xb0'''

    In [36]: type(b1)
    Out[36]: bytes

    In [37]: len(b1)
    Out[37]: 4

In Python, bytes that correspond to ASCII symbols are displayed as these
symbols, not as their corresponding bytes. This may be a bit confusing
but it is always possible to recognize bytes type by letter ``b``:

.. code:: python

    In [38]: bytes1 = b'hello'

    In [39]: bytes1
    Out[39]: b'hello'

    In [40]: len(bytes1)
    Out[40]: 5

    In [41]: bytes1.hex()
    Out[41]: '68656c6c6f'

    In [42]: bytes2 = b'\x68\x65\x6c\x6c\x6f'

    In [43]: bytes2
    Out[43]: b'hello'

If you try to write not an ASCII character in a byte literal,
an error will occur:

.. code:: python

    In [44]: bytes3 = b'привет'
      File "<ipython-input-44-dc8b23504fa7>", line 1
        bytes3 = b'привет'
                ^
    SyntaxError: bytes can only contain ASCII literal characters.

