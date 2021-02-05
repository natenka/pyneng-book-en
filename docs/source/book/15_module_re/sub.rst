Function re.sub
--------------

Function ``re.sub`` works similary to ``replace`` method in strings. But in
``re.sub`` you can use regex and therefore make substitutions using more complex conditions.
Replace commas, square brackets and via word with space in ospf_route string:

.. code:: python

    In [7]: ospf_route = 'O    10.0.24.0/24 [110/41] via 10.0.13.3, 3d18h, FastEthernet0/0'

    In [8]: re.sub(r'(via|[,\[\]])', ' ', ospf_route)
    Out[8]: 'O        10.0.24.0/24  110/41    10.0.13.3  3d18h  FastEthernet0/0'

With ``re.sub`` you can transform a string. For example, convert mac_table string to:

.. code:: python

    In [9]: mac_table = '''
       ...:  100    aabb.cc10.7000    DYNAMIC     Gi0/1
       ...:  200    aabb.cc20.7000    DYNAMIC     Gi0/2
       ...:  300    aabb.cc30.7000    DYNAMIC     Gi0/3
       ...:  100    aabb.cc40.7000    DYNAMIC     Gi0/4
       ...:  500    aabb.cc50.7000    DYNAMIC     Gi0/5
       ...:  200    aabb.cc60.7000    DYNAMIC     Gi0/6
       ...:  300    aabb.cc70.7000    DYNAMIC     Gi0/7
       ...: '''

    In [4]: print(re.sub(r' *(\d+) +'
       ...:              r'([a-f0-9]+)\.'
       ...:              r'([a-f0-9]+)\.'
       ...:              r'([a-f0-9]+) +\w+ +'
       ...:              r'(\S+)',
       ...:              r'\1 \2:\3:\4 \5',
       ...:              mac_table))
       ...:

    100 aabb:cc10:7000 Gi0/1
    200 aabb:cc20:7000 Gi0/2
    300 aabb:cc30:7000 Gi0/3
    100 aabb:cc40:7000 Gi0/4
    500 aabb:cc50:7000 Gi0/5
    200 aabb:cc60:7000 Gi0/6
    300 aabb:cc70:7000 Gi0/7

Regex is divided into groups:

-  ``(\d+)`` - the first group. VLAN number gets here
-  ``([a-f,0-9]+).([a-f,0-9]+).([a-f,0-9]+)`` - the following three groups (2, 3, 4) describe MAC address
-  ``(\S+)`` - the fifth group. Describes an interface.

In a second regex these groups are used. To refer to a group a backslash and a
group number are used. To avoid backslash screening, raw string is used.
As a result, the corresponding substrings will be substituted instead of group
numbers. For example, format of MAC address record was also changed.
