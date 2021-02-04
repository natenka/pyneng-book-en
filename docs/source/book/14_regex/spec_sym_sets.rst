Character sets
---------------

Python has special designations for character sets:

*  ``\d`` - any digit
*  ``\D`` - any non-numeric value
*  ``\s`` - whitespace character
*  ``\S`` - all except whitespace characters
*  ``\w`` - any letter, digit or underline character
*  ``\W`` - all except letter, digit or underline character

.. note::

    These are not all character sets that support Python. See 
    `documentation <https://docs.python.org/3/library/re.html>`__ for details.

Character sets allow you to write shorter expressions without having to list
all necessary characters.
For example, get time from log file string:

.. code:: python

    In [1]: log = '*Jul  7 06:15:18.695: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/3, changed state to down'

    In [2]: re.search('\d\d:\d\d:\d\d', log).group()
    Out[2]: '06:15:18'

Expression ``\d\d:\d\d:\d\d`` describes 3 pairs of numbers separated by colons.

Getting MAC address from log message:

.. code:: python

    In [3]: log2 = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [4]: re.search('\w\w\w\w\.\w\w\w\w\.\w\w\w\w', log2).group()
    Out[4]: 'f03a.b216.7ad7'

Expression ``\w\w\w\w\.\w\w\w\w\.\w\w\w\w`` describes 12 letters or digits that
are divided into three groups of four characters and separated by dot. 

Symbol groups are very convenient, but for now it is necessary to manually
specify a character repetition. The following subsection covers repetition
symbols which will simplify description of expressions.
