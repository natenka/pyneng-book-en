Conversion between bytes and strings
------------------------------------

You can't avoid working with bytes. For example, when working with a network
or a filesystem, most often the result is returned in bytes.
Accordingly, you need to know how to convert bytes to string and vice versa.
That's what the encoding is for.

Encoding can be represented as an encryption key that specifies:

* how to "encrypt" a string to bytes (str -> bytes). Encode method used (similar to encrypt)
* how to "decrypt" bytes to string (bytes -> str). Decode method used (similar to decrypt)

This analogy makes it clear that string-byte and byte-string transformations
must use the same encoding.

``encode``, ``decode``
~~~~~~~~~~~~~~~~~~~~~~

``encode`` method is used to convert string to bytes:

.. code:: python

    In [1]: hi = 'привет'

    In [2]: hi.encode('utf-8')
    Out[2]: b'\xd0\xbf\xd1\x80\xd0\xb8\xd0\xb2\xd0\xb5\xd1\x82'

    In [3]: hi_bytes = hi.encode('utf-8')

``decode`` method to get a string from bytes:

.. code:: python

    In [4]: hi_bytes
    Out[4]: b'\xd0\xbf\xd1\x80\xd0\xb8\xd0\xb2\xd0\xb5\xd1\x82'

    In [5]: hi_bytes.decode('utf-8')
    Out[5]: 'привет'

``str.encode``, ``bytes.decode``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Method ``encode`` is also present in str class (as are other methods of
working with strings):

.. code:: python

    In [6]: hi
    Out[6]: 'привет'

    In [7]: str.encode(hi, encoding='utf-8')
    Out[7]: b'\xd0\xbf\xd1\x80\xd0\xb8\xd0\xb2\xd0\xb5\xd1\x82'

And ``decode`` method is available in bytes class (like other methods):

.. code:: python

    In [8]: hi_bytes
    Out[8]: b'\xd0\xbf\xd1\x80\xd0\xb8\xd0\xb2\xd0\xb5\xd1\x82'

    In [9]: bytes.decode(hi_bytes, encoding='utf-8')
    Out[9]: 'привет'

In these methods, encoding can be used as a key argument (examples above)
or as a positional argument:

.. code:: python

    In [10]: hi_bytes
    Out[10]: b'\xd0\xbf\xd1\x80\xd0\xb8\xd0\xb2\xd0\xb5\xd1\x82'

    In [11]: bytes.decode(hi_bytes, 'utf-8')
    Out[11]: 'привет'

How to work with Unicode and bytes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There is a rule called a "Unicode sandwich":

* bytes that the program reads must be converted to Unicode (string) as early as possible
* inside the program work with Unicode 
* Unicode must be converted to bytes as soon as possible before transmitting


