Non-capturing group
------------------

By default, everything that fell into the group is remembered. It’s called a
capturing group.

Sometimes parentheses are needed to indicate a part of expression that repeats.
And, in doing so, you don’t need to remember an expression.

For example, get a MAC address, VLAN and ports from log message:

.. code:: python

    In [1]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

A regular expression that describes substrings needed:

.. code:: python

    In [2]: match = re.search('((\w{4}\.){2}\w{4}).+vlan (\d+).+port (\S+).+port (\S+)', log)

Expression consists of the following parts:

* ``((\w{4}\.){2}\w{4})`` - MAC address gets here 
* ``\w{4}\.`` - this part describes 4 letters or digits and a dot
* ``(\w{4}\.){2}`` - here parentheses are used to indicate that 4 letters or digits and a dot are repeated twice
* ``\w{4}`` - then 4 letters or numbers
* ``.+vlan (\d+)`` - VLAN number falls into the group 
* ``.+port (\S+)`` - first interface
* ``.+port (\S+)`` - second interface

Method ``groups`` returns:

.. code:: python

    In [3]: match.groups()
    Out[3]: ('f03a.b216.7ad7', 'b216.', '10', 'Gi0/5', 'Gi0/15')

The second element is essentially superfluous. It appeared in the output because
of parentheses in expression ``(\w{4}\.){2}``.

In this case, you need to disable capture in the group. This is done by
adding ``?:`` after the group's opening parenthesis.

Now the expression looks like this:

.. code:: python

    In [4]: match = re.search('((?:\w{4}\.){2}\w{4}).+vlan (\d+).+port (\S+).+port (\S+)', log)

Accordingly, ``groups`` method returns:

.. code:: python

    In [5]: match.groups()
    Out[5]: ('f03a.b216.7ad7', '10', 'Gi0/5', 'Gi0/15')

