Method chaining
===============

Often, you need to perform several operations with data, for example:

.. code:: python

    In [1]: line = "switchport trunk allowed vlan 10,20,30"

    In [2]: words = line.split()

    In [3]: words
    Out[3]: ['switchport', 'trunk', 'allowed', 'vlan', '10,20,30']

    In [4]: vlans_str = words[-1]

    In [5]: vlans_str
    Out[5]: '10,20,30'

    In [6]: vlans = vlans_str.split(",")

    In [7]: vlans
    Out[7]: ['10', '20', '30']

In the script:

.. code:: python

    line = "switchport trunk allowed vlan 10,20,30"
    words = line.split()
    vlans_str = words[-1]
    vlans = vlans_str.split(",")
    print(vlans)

In this case, variables are used to store the intermediate result
and subsequent methods/actions are performed with the variable.
This is a completely normal version of the code, especially at first when it's hard
perceive more complex expressions.

However, in Python, there are often expressions in which actions or methods
are applied one after the other in one expression.
For example, the previous code could be written like this:

.. code:: python

    line = "switchport trunk allowed vlan 10,20,30"
    vlans = line.split()[-1].split(",")
    print(vlans)

Since there are no expressions in parentheses that would indicate the priority of execution,
everything is executed from left to right.


First, ``line.split()`` is executed - we get the list, then to the resulting list
applies ``[-1]`` - we get the last element of the list, the line ``10,20,30``.
The method ``split(",")`` is applied to this line and as a result we get the list ``['10', '20', '30']``.


The main nuance when writing such chains, the previous method/action should return something
what the next method/action is waiting for.
And it is imperative that something is returned, otherwise there will be an error.
