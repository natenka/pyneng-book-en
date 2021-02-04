Special symbols
-------------------

*  ``.`` - any character except line feed character
*  ``^`` - beginning of line
*  ``$`` - end of line
*  ``[abc]`` - any symbol in brackets
*  ``[^abc]`` - any symbol except those in brackets
*  ``a|b`` - element a or b
*  ``(regex)`` - expression is treated as one element. In addition, substring that matches an expression is memorized

``.``
~~~~~

Dot represents any symbol.
Most often, a dot is used with repetition symbols ``+`` and ``*`` to indicate
that any character can be found between certain expressions.

For example, using expression ``Interface.+Port ID.+`` you can describe a line
with interfaces in the output "sh cdp neighbors detail":

.. code:: python

    In [1]: cdp = '''
       ...: SW1#show cdp neighbors detail
       ...: -------------------------
       ...: Device ID: SW2
       ...: Entry address(es):
       ...:   IP address: 10.1.1.2
       ...: Platform: cisco WS-C2960-8TC-L,  Capabilities: Switch IGMP
       ...: Interface: GigabitEthernet1/0/16,  Port ID (outgoing port): GigabitEthernet0/1
       ...: Holdtime : 164 sec
       ...: '''

    In [2]: re.search('Interface.+Port ID.+', cdp).group()
    Out[2]: 'Interface: GigabitEthernet1/0/16,  Port ID (outgoing port): GigabitEthernet0/1'

The result was only one string as the dot represents any character except line
feed character. In addition, repetition characters  ``+`` and ``*`` by default
capture the longest string possible. This aspect is addressed in subsection "Greedy qualifiers".

``^``
~~~~~
Character  ``^`` means the beginning of line. Expression ``^\d+`` corresponds to substring:

.. code:: python

    In [3]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [4]: re.search('^\d+', line).group()
    Out[4]: '100'

Characters from beginning of line to pound sign (including pound):

.. code:: py

    In [5]: prompt = 'SW1#show cdp neighbors detail'

    In [6]: re.search('^.+#', prompt).group()
    Out[6]: 'SW1#'

``$``
~~~~~

Symbol ``$`` represents the end of a line.

Expression ``\S+$`` describes any characters except whitespace at the end of line:

.. code:: python

    In [7]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [8]: re.search('\S+$', line).group()
    Out[8]: 'FastEthernet0/1'

``[]``
~~~~~~

Symbols that are listed in square brackets mean that any of these symbols will
be a match. Thus, different registers can be described:

.. code:: python

    In [9]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [10]: re.search('[Ff]ast', line).group()
    Out[10]: 'Fast'

    In [11]: re.search('[Ff]ast[Ee]thernet', line).group()
    Out[11]: 'FastEthernet'

Using square brackets, you can specify which characters may meet at a specific position. For example, expression ``^.+[>#]`` describes characters from the beginning of a line to # or > sign (including them). This expression can be used to derive the name of device:

.. code:: python

    In [12]: commands = ['SW1#show cdp neighbors detail',
        ...:             'SW1>sh ip int br',
        ...:             'r1-london-core# sh ip route']
        ...:

    In [13]: for line in commands:
        ...:     match = re.search('^.+[>#]', line)
        ...:     if match:
        ...:         print(match.group())
        ...:
    SW1#
    SW1>
    r1-london-core#

You can specify character ranges in square brackets. For example,
any number from 0 to 9:

.. code:: python

    In [14]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [15]: re.search('[0-9]+', line).group()
    Out[15]: '100'

Letters:

.. code:: python

    In [16]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [17]: re.search('[a-z]+', line).group()
    Out[17]: 'aa'

    In [18]: re.search('[A-Z]+', line).group()
    Out[18]: 'F'

Several ranges may be indicated in square brackets:

.. code:: py

    In [19]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [20]: re.search('[a-f0-9]+\.[a-f0-9]+\.[a-f0-9]+', line).group()
    Out[20]: 'aa12.35fe.a5d3'

Expression ``[a-f0-9]+\.[a-f0-9]+\.[a-f0-9]+`` describes three groups of
symbols separated by a dot. Characters in each group can be letters a-f
or digits 0-9. This expression describes MAC address.

Another feature of square brackets is that the special symbols within
square brackets lose their special meaning and are simply a symbol.
For example, a dot inside square brackets will denote a dot, not any symbol.

Expression ``[a-f0-9]+[./][a-f0-9]+`` describes three groups of symbols:

1. letters a-f or digits 0-9
2. dot or slash
3. letters a-f or digits 0-9

For ``line`` string the match will be a such substring:

.. code:: python

    In [21]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [22]: re.search('[a-f0-9]+[./][a-f0-9]+', line).group()
    Out[22]: 'aa12.35fe'

If first symbol in square brackets is ``^``, match will be any symbol except those in brackets.

.. code:: python

    In [23]: line = 'FastEthernet0/0    15.0.15.1       YES manual up         up'

    In [24]: re.search('[^a-zA-Z]+', line).group()
    Out[24]: '0/0    15.0.15.1       '

In this case, expression describes everything except letters.

``|``
~~~~~

Pipe symbol works like 'or':

.. code:: python

    In [25]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [26]: re.search('Fast|0/1', line).group()
    Out[26]: 'Fast'

Note how ``|`` works - Fast и 0/1 are treated as an whole expression.
So in the end, expression means that we’re looking for Fast or 0/1.

``()``
~~~~~~

Brackets are used to group expressions. As in mathematical expressions,
brackets can be used to indicate which elements the operation is applied to.

For example, expression ``[0-9]([a-f]|[0-9])[0-9]`` describes three characters: digit, then a letter or digit and digit:

.. code:: python

    In [27]: line = "100     aa12.35fe.a5d3    FastEthernet0/1"

    In [28]: re.search('[0-9]([a-f]|[0-9])[0-9]', line).group()
    Out[28]: '100'

Brackets allow to indicate which expression is a one entity. This is
particularly useful when using repetition symbols:

.. code:: python

    In [29]: line = 'FastEthernet0/0    15.0.15.1       YES manual up         up'

    In [30]: re.search('([0-9]+\.)+[0-9]+', line).group()
    Out[30]: '15.0.15.1'

Brackets not only allow you to group expressions. String that matches bracketed
expression is memorized. It can be obtained separately by special methods
``groups`` and ``group(n)``. This is covered in subsection "Grouping".
