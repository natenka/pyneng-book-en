Exception Handling
------------------

Let's see an example of how to use ``execute`` method when an error occurs.

In switch table the mac field must be unique. If you try to write an
overlapping MAC address, there is an error:

.. code:: python

    In [37]: con = sqlite3.connect('sw_inventory2.db')

    In [38]: query = "INSERT into switch values ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960', 'London, Green Str')"

    In [39]: con.execute(query)
    ------------------------------------------------------------
    IntegrityError             Traceback (most recent call last)
    <ipython-input-56-ad34d83a8a84> in <module>()
    ----> 1 con.execute(query)

    IntegrityError: UNIQUE constraint failed: switch.mac

Accordingly, you can catch the exception:

.. code:: python

    In [40]: try:
        ...:     con.execute(query)
        ...: except sqlite3.IntegrityError as e:
        ...:     print("Error occurred: ", e)
        ...:
    Error occurred:  UNIQUE constraint failed: switch.mac

Note that you should catch ``sqlite3.IntegrityError`` exception,
not ``IntegrityError``.
