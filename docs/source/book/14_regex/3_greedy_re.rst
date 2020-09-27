Greedy symbols
----------------------------

By default, repetition symbols in regular expressions are greedy. This means that the resulting substring which corresponds to the template will have the longest match.

An example of greedy behavior:

.. code:: python

    In [1]: import re
    In [2]: line = '<text line> some text>'
    In [3]: match = re.search('<.*>', line)

    In [4]: match.group()
    Out[4]: '<text line> some text>'

That is, in this case, the expression captured the maximum possible piece of symbols contained in <>.

If greedy behavior need to be disabled, it is sufficient to add a question mark after the repetition symbols:

.. code:: python

    In [5]: line = '<text line> some text>'

    In [6]: match = re.search('<.*?>', line)

    In [7]: match.group()
    Out[7]: '<text line>'

But greed is often useful. For example, without turning off the greed of the last plus, the expression ``\d+\s+\S+`` describes such a line:

.. code:: python

    In [8]: line = '1500     aab1.a1a1.a5d3    FastEthernet0/1'

    In [9]: re.search('\d+\s+\S+', line).group()
    Out[9]: '1500     aab1.a1a1.a5d3'

Symbol ``\S`` denotes everything except whitespace characters. Therefore, the expression ``\S+`` with the greedy repetition symbol describes the maximal long string until the first whitespace character. In this case up to the first space.

If greed is disabled, the result is:

.. code:: python

    In [10]: re.search('\d+\s+\S+?', line).group()
    Out[10]: '1500     a'

