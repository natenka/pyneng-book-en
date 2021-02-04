Greedy qualifiers
-----------------

By default, ``*``, ``+``, and ``?`` qualifiers are all greedy - they match
as much text as possible.

An example of greedy behavior:

.. code:: python

    In [1]: import re

    In [2]: line = '<text line> some text>'

    In [3]: match = re.search('<.*>', line)

    In [4]: match.group()
    Out[4]: '<text line> some text>'

In this case, expression captured maximum possible piece of symbols contained
in ``<>``. If greedy behavior need to be disabled, just add a question mark
after the repetition symbols:

.. code:: python

    In [5]: line = '<text line> some text>'

    In [6]: match = re.search('<.*?>', line)

    In [7]: match.group()
    Out[7]: '<text line>'

But greed is often useful. For example, without turning off greed of the last
plus, expression ``\d+\s+\S+`` describes line:

.. code:: python

    In [8]: line = '1500     aab1.a1a1.a5d3    FastEthernet0/1'

    In [9]: re.search('\d+\s+\S+', line).group()
    Out[9]: '1500     aab1.a1a1.a5d3'

Symbol ``\S`` denotes everything except whitespace characters. Therefore,
expression ``\S+`` with greedy repetition symbol describes maximum long
string until the first whitespace character. In this case up to the first space.

If greed is disabled, the result is:

.. code:: python

    In [10]: re.search('\d+\s+\S+?', line).group()
    Out[10]: '1500     a'

