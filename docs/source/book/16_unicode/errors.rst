Converting errors
----------------------

When converting between strings and bytes it is very important to know exactly
which encoding is used as well as to know the possibilities of different encodings.

For example, ASCII codec cannot encode Cyrillic:

.. code:: python

    In [32]: hi_unicode = 'привет'

    In [33]: hi_unicode.encode('ascii')
    ---------------------------------------------------------------------------
    UnicodeEncodeError                        Traceback (most recent call last)
    <ipython-input-33-ec69c9fd2dae> in <module>()
    ----> 1 hi_unicode.encode('ascii')

    UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-5: ordinal not in range(128)

Similarly, if the string "привет" is converted to bytes and you try to convert
it into a string with ascii, we will also get an error:

.. code:: python


    In [34]: hi_unicode = 'привет'

    In [35]: hi_bytes = hi_unicode.encode('utf-8')

    In [36]: hi_bytes.decode('ascii')
    ---------------------------------------------------------------------------
    UnicodeDecodeError                        Traceback (most recent call last)
    <ipython-input-36-aa0ada5e44e9> in <module>()
    ----> 1 hi_bytes.decode('ascii')

    UnicodeDecodeError: 'ascii' codec can't decode byte 0xd0 in position 0: ordinal not in range(128)

Another version of error where different encodings are used to conversion:

.. code:: python


    In [37]: de_hi_unicode = 'grüezi'

    In [38]: utf_16 = de_hi_unicode.encode('utf-16')

    In [39]: utf_16.decode('utf-8')
    ---------------------------------------------------------------------------
    UnicodeDecodeError                        Traceback (most recent call last)
    <ipython-input-39-4b4c731e69e4> in <module>()
    ----> 1 utf_16.decode('utf-8')

    UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte

Having mistakes is good. They're telling what the problem is.
It's worse when it's like this:

.. code:: python

    In [40]: hi_unicode = 'привет'

    In [41]: hi_bytes = hi_unicode.encode('utf-8')

    In [42]: hi_bytes
    Out[42]: b'\xd0\xbf\xd1\x80\xd0\xb8\xd0\xb2\xd0\xb5\xd1\x82'

    In [43]: hi_bytes.decode('utf-16')
    Out[43]: '뿐胑룐닐뗐苑'

Error processing
~~~~~~~~~~~~~~~~

Encode and decode methods have error-processing modes that indicate how
to respond to a conversion error.

Parameter errors in encode
^^^^^^^^^^^^^^^^^^^^^^^^^^

By default ``encode`` uses ``strict`` mode - UnicodeError exception is
generated when encoding errors occur. Examples of such behaviour are above.

Instead, you can use ``replace`` to substitute character with a question mark:

.. code:: python

    In [44]: de_hi_unicode = 'grüezi'

    In [45]: de_hi_unicode.encode('ascii', 'replace')
    Out[45]: b'gr?ezi'

Or ``namereplace`` to replace character with the name:

.. code:: python

    In [46]: de_hi_unicode = 'grüezi'

    In [47]: de_hi_unicode.encode('ascii', 'namereplace')
    Out[47]: b'gr\\N{LATIN SMALL LETTER U WITH DIAERESIS}ezi'

In addition, characters that cannot be encoded may be completely ignored:

.. code:: python

    In [48]: de_hi_unicode = 'grüezi'

    In [49]: de_hi_unicode.encode('ascii', 'ignore')
    Out[49]: b'grezi'

Parameter errors in decode
^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``decode`` method also uses strict mode by default and generates
a UnicodeDecodeError exception.

If you change mode to ignore, as in encode, characters will simply be ignored:

.. code:: python

    In [50]: de_hi_unicode = 'grüezi'

    In [51]: de_hi_utf8 = de_hi_unicode.encode('utf-8')

    In [52]: de_hi_utf8
    Out[52]: b'gr\xc3\xbcezi'

    In [53]: de_hi_utf8.decode('ascii', 'ignore')
    Out[53]: 'grezi'

Mode ``replace`` substitutes characters:

.. code:: python

    In [54]: de_hi_unicode = 'grüezi'

    In [55]: de_hi_utf8 = de_hi_unicode.encode('utf-8')

    In [56]: de_hi_utf8.decode('ascii', 'replace')
    Out[56]: 'gr��ezi'

