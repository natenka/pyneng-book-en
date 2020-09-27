Compile function
---------------

Python has the ability to pre-compile a regular expression and then use it. This is particularly useful when regular expression is used a lot in the script.

The use of a compiled expression can speed up processing and it is generally more convenient to use this option as the program divides the creation of a regular expression and its use. In addition, using re.compile function creates a RegexObject object that has several additional features that are not present in the MatchObject object.

To compile a regular expression, use re.compile:

.. code:: python

    In [52]: regex = re.compile(r'\d+ +\S+ +\w+ +\S+')

It returns the RegexObject object:

.. code:: python

    In [53]: regex
    Out[53]: re.compile(r'\d+ +\S+ +\w+ +\S+', re.UNICODE)

RegexObject has such methods and attributes:

.. code:: python

    In [55]: [method for method in dir(regex) if not method.startswith('_')]
    Out[55]:
    ['findall',
     'finditer',
     'flags',
     'fullmatch',
     'groupindex',
     'groups',
     'match',
     'pattern',
     'scanner',
     'search',
     'split',
     'sub',
     'subn']

Note that Regex object has search(), match(), finditer(), findall() methods available. These are the same functions that are available in the module globally, but now they have to be applied to the object.

An example of using search() method:

.. code:: python

    In [67]: line = ' 100    a1b2.ac10.7000    DYNAMIC     Gi0/1'

    In [68]: match = regex.search(line)

Now search() should be called as the method of *regex* object. And pass the string as an argument.

The result is a Match object:

.. code:: python

    In [69]: match
    Out[69]: <_sre.SRE_Match object; span=(1, 43), match='100    a1b2.ac10.7000    DYNAMIC     Gi0/1'>

    In [70]: match.group()
    Out[70]: '100    a1b2.ac10.7000    DYNAMIC     Gi0/1'

An example of compiling a regular expression and its use based on example of a log file (parse_log_compile.py file):

.. code:: python

    import re

    regex = re.compile(r'Host \S+ '
                       r'in vlan (\d+) '
                       r'is flapping between port '
                       r'(\S+) and port (\S+)')

    ports = set()

    with open('log.txt') as f:
        for m in regex.finditer(f.read()):
            vlan = m.group(1)
            ports.add(m.group(2))
            ports.add(m.group(3))

    print('Петля между портами {} в VLAN {}'.format(', '.join(ports), vlan))

This is a modified example of finditer() usage. Description of regular expression changed:

.. code:: python

    regex = re.compile(r'Host \S+ '
                       r'in vlan (\d+) '
                       r'is flapping between port '
                       r'(\S+) and port (\S+)')

And now the call of finditer() is executed as a *regex* object method:

.. code:: python

        for m in regex.finditer(f.read()):

Options that are available only when using re.compile
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When using re.compile in search(), match(), findall(), finditer() and fullmatch() methods, additional parameters appear:

* pos - allows you to specify an index in the string from where to start looking for a match
* endpos - specifies from which index the search should be started

Their use is similar to the execution of a string slice.

For example, this is the result without specifying *pos*, *endpos* parameters:

.. code:: python

    In [75]: regex = re.compile(r'\d+ +\S+ +\w+ +\S+')

    In [76]: line = ' 100    a1b2.ac10.7000    DYNAMIC     Gi0/1'

    In [77]: match = regex.search(line)

    In [78]: match.group()
    Out[78]: '100    a1b2.ac10.7000    DYNAMIC     Gi0/1'

In this case, the initial search position should be indicated:

.. code:: python

    In [79]: match = regex.search(line, 2)

    In [80]: match.group()
    Out[80]: '00    a1b2.ac10.7000    DYNAMIC     Gi0/1'

The initial entry is the same as string slice:

.. code:: python

    In [81]: match = regex.search(line[2:])

    In [82]: match.group()
    Out[82]: '00    a1b2.ac10.7000    DYNAMIC     Gi0/1'

A final example is the use of two indexes:

.. code:: python

    In [90]: line = ' 100    a1b2.ac10.7000    DYNAMIC     Gi0/1'

    In [91]: regex = re.compile(r'\d+ +\S+ +\w+ +\S+')

    In [92]: match = regex.search(line, 2, 40)

    In [93]: match.group()
    Out[93]: '00    a1b2.ac10.7000    DYNAMIC     Gi'

And a similar string slice:

.. code:: python

    In [94]: match = regex.search(line[2:40])

    In [95]: match.group()
    Out[95]: '00    a1b2.ac10.7000    DYNAMIC     Gi'

In match(), findall(), finditer() and fullmatch() methods *pos* and *endpos* parameters work similarly.

