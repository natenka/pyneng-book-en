Function re.split
----------------

Function ``split`` works similary to ``split`` method in strings,
but in ``re.split`` function you can use regular expressions which
means dividing a string into parts using more complex conditions.

For example, ``ospf_route`` string should be split by spaces
(as in ``str.split`` method):

.. code:: python

    In [1]: ospf_route = 'O     10.0.24.0/24 [110/41] via 10.0.13.3, 3d18h, FastEthernet0/0'

    In [2]: re.split(r' +', ospf_route)
    Out[2]:
    ['O',
     '10.0.24.0/24',
     '[110/41]',
     'via',
     '10.0.13.3,',
     '3d18h,',
     'FastEthernet0/0']

Similarly, commas can be removed:

.. code:: python

    In [3]: re.split(r'[ ,]+', ospf_route)
    Out[3]:
    ['O',
     '10.0.24.0/24',
     '[110/41]',
     'via',
     '10.0.13.3',
     '3d18h',
     'FastEthernet0/0']

And if necessary, get rid of square brackets:

.. code:: python

    In [4]: re.split(r'[ ,\[\]]+', ospf_route)
    Out[4]: ['O', '10.0.24.0/24', '110/41', 'via', '10.0.13.3', '3d18h', 'FastEthernet0/0']

Function ``split`` has a peculiarity of working with groups (expressions in
parentheses). If you specify the same expression with parentheses, the resulting
list will include separators.

For example, word via is specified as a separator:

.. code:: python

    In [5]: re.split(r'(via|[ ,\[\]])+', ospf_route)
    Out[5]:
    ['O',
     ' ',
     '10.0.24.0/24',
     '[',
     '110/41',
     ' ',
     '10.0.13.3',
     ' ',
     '3d18h',
     ' ',
     'FastEthernet0/0']

To disable such behavior you should make a noncapture group. That is,
disable capturing of group elements:

.. code:: python

    In [6]: re.split(r'(?:via|[ ,\[\]])+', ospf_route)
    Out[6]: ['O', '10.0.24.0/24', '110/41', '10.0.13.3', '3d18h', 'FastEthernet0/0']

