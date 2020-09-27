Regular expression syntax
------------------------------

Python uses **re** module to work with regular expressions. Accordingly, you have to  import it to start working with regular expressions.

This section will use the search() function for all examples. And in the next subsection, the rest of the functions of **re** will be considered.

Syntax of the search() function is:

.. code:: python

    match = re.search(regex, string)

The search() function has two prerequisites:

* regex - regular expression
* string - string in which search pattern is searched

If a match is found the function will return special object Match. If there is no match, the function will return None.

The feature of the search() function is that it only looks for a first match. That is, if there are several substrings in a line that correspond to a regular expression, search() will return only the first match found.

To get an idea of regular expressions, consider a few examples.

The simplest example of a regular expression is a substring:

.. code:: python

    In [1]: import re

    In [2]: int_line = '  MTU 1500 bytes, BW 10000 Kbit, DLY 1000 usec,'

    In [3]: match = re.search('MTU', int_line)

In this example:

* first import module **re**
* then goes the example of  string  - int_line 
* •	and in line 3 the search pattern is passed to search() function plus string int_line in which the match is searched

In this case we are simply looking for whether there is ‘MTU’ substring in string int_line.

If it exists, the *match* variable will contain a special Match object:

.. code:: python

    In [4]: print(match)
    <_sre.SRE_Match object; span=(2, 5), match='MTU'>

Match object has several methods that allow to get different information about the received match. For example, the group() method shows that the string matches the expression described.

In this case, it’s just a ‘MTU’ substring:

.. code:: python

    In [5]: match.group()
    Out[5]: 'MTU'

If there was no match, the *match* variable will have None value:

.. code:: python

    In [6]: int_line = '  MTU 1500 bytes, BW 10000 Kbit, DLY 1000 usec,'

    In [7]: match = re.search('MU', int_line)

    In [8]: print(match)
    None

Regular expressions are fully enabled when special characters are used. For example, the symbol ``\d`` means a digit, but ``+``
means repetition of the previous symbol one or more times. If you combine them ``\d+``, you get an expression that means one or more digits.

Using this expression, you can get the part of the string that describes the bandwidth:

.. code:: python

    In [9]: int_line = '  MTU 1500 bytes, BW 10000 Kbit, DLY 1000 usec,'

    In [10]: match = re.search('BW \d+', int_line)

    In [11]: match.group()
    Out[11]: 'BW 10000'

Regular expressions are particularly useful in getting certain substrings from the string. For example, it is necessary to get VLAN, MAC and ports from the output of such log message:

.. code:: python

    In [12]: log2 = 'Oct  3 12:49:15.941: %SW_MATM-4-MACFLAP_NOTIF: Host f04d.a206.7fd6 in vlan 1 is flapping between port Gi0/5 and port Gi0/16'

This can be done through the regular expression:

.. code:: python

    In [13]: re.search('Host (\S+) in vlan (\d+) is flapping between port (\S+) and port (\S+)', log2).groups()
    Out[13]: ('f04d.a206.7fd6', '1', 'Gi0/5', 'Gi0/16')

The group() method returns only those parts of the original string that are in brackets. Thus, by placing a part of the expression in brackets, you can specify which parts of the line you want to remember.

The expression ``\d+`` has been used before - it describes one or more digits.  And the expression ``\S+`` describes all characters except whitespace (space, tab, etc.).

The following subsections deal with special characters that are used in regular expressions.

.. note::

    If you know what special characters mean in regular expressions, you can skip the following subsection and immediately switch to the subsection about module **re**.
