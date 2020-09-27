Non-capturing group
------------------

By default, everything that fell into the group is remembered. It’s called a capturing group.

Sometimes brackets are needed to indicate the part of the expression that repeats. And, in doing so, you don’t need to remember the expression.

For example, get a MAC address, VLAN and ports from such log message:

.. code:: python

    In [1]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

A regular expression that describes the substrings needed:

.. code:: python

    In [2]: match = re.search('((\w{4}\.){2}\w{4}).+vlan (\d+).+port (\S+).+port (\S+)', log)

The expression consists of the following parts:

* ``((\w{4}\.){2}\w{4})`` - MAC address gets here 
* ``\w{4}\.`` - this part describes 4 letters or digits and a dot
* ``(\w{4}\.){2}`` - here the brackets are used to indicate that 4 letters or digits and a dot are repeated twice
* ``\w{4}`` - then 4 letters or numbers
* ``.+vlan (\d+)`` - VLAN number falls into the group 
* ``.+port (\S+)`` - the first interface
* ``.+port (\S+)`` - the second interface

The groups() method returns this result:

.. code:: python

    In [3]: match.groups()
    Out[3]: ('f03a.b216.7ad7', 'b216.', '10', 'Gi0/5', 'Gi0/15')

The second element is essentially superfluous. It appeared in the output because of the brackets in the expression ``(\w{4}\.){2}``.

In that case, we need to disable the group capturing. This is done by adding 
``?:`` after the group bracket opens.

Now the expression looks like this:

.. code:: python

    In [4]: match = re.search('((?:\w{4}\.){2}\w{4}).+vlan (\d+).+port (\S+).+port (\S+)', log)

Accordingly, the groups() method result:

.. code:: python

    In [5]: match.groups()
    Out[5]: ('f03a.b216.7ad7', '10', 'Gi0/5', 'Gi0/15')

