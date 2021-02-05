String formatting with ``%`` operator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example of ``%`` operator use:

.. code:: python

    In [2]: "interface FastEthernet0/%s" % '1'
    Out[2]: 'interface FastEthernet0/1'

Old string format syntax uses these symbols:

* ``%s`` - string or any other object with a string type
* ``%d`` - integer
* ``%f`` - float

Output data columns of equal width of 15 characters with right side alignment:

.. code:: python

    In [3]: vlan, mac, intf = ['100', 'aabb.cc80.7000', 'Gi0/1']

    In [4]: print("%15s %15s %15s" % (vlan, mac, intf))
                100  aabb.cc80.7000           Gi0/1

Left side alignment:

.. code:: python

    In [6]: print("%-15s %-15s %-15s" % (vlan, mac, intf))
    100             aabb.cc80.7000  Gi0/1

You can also use string formatting to influence the appearance of numbers.

For example, you can specify how many digits to show after comma:

.. code:: python

    In [8]: print("%.3f" % (10.0/3))
    3.333

.. note::
    String formatting still has many possibilities. Good examples and
    explanations of two string formatting options can be found
    `here <https://pyformat.info/>`__.
