Match function
-------------

Function ``match``: 

* is used to search at the beginning of string that corresponds to template
* returns Match object if substring is found
* returns ``None`` if no substring was found

``Match`` function differs from ``search`` in that ``match`` always looks for a
match at the beginning of the line. For example, if you repeat the example that
was used for ``search`` function, but with ``match``:

.. code:: python

    In [2]: import re

    In [3]: log = '%SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in vlan 10 is flapping between port Gi0/16 and port Gi0/24'

    In [4]: match = re.match(r'Host \S+ '
       ...:                  r'in vlan (\d+) '
       ...:                  r'is flapping between port '
       ...:                  r'(\S+) and port (\S+)', log)
       ...:

The result will be None:

.. code:: python

    In [6]: print(match)
    None

This is because match() searches for *Host* word at the beginning of the line. But this message is in the middle.

In this case it is easy to fix expression so that match() function finds match:

.. code:: python

    In [4]: match = re.match(r'\S+: Host \S+ '
       ...:                  r'in vlan (\d+) '
       ...:                  r'is flapping between port '
       ...:                  r'(\S+) and port (\S+)', log)
       ...:

Expression ``\S+:`` was added before *Host* word. Now match will be found:

.. code:: python

    In [11]: print(match)
    <_sre.SRE_Match object; span=(0, 104), match='%SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in >

    In [12]: match.groups()
    Out[12]: ('10', 'Gi0/16', 'Gi0/24')

Example is similar to one used in ``search`` function with minor changes
(parse_log_match match.py file):

.. code:: python

    import re

    regex = (r'\S+: Host \S+ '
             r'in vlan (\d+) '
             r'is flapping between port '
             r'(\S+) and port (\S+)')

    ports = set()

    with open('log.txt') as f:
        for line in f:
            match = re.match(regex, line)
            if match:
                vlan = match.group(1)
                ports.add(match.group(2))
                ports.add(match.group(3))

    print('Loop between ports {} в VLAN {}'.format(', '.join(ports), vlan))

The result is:

::

    $ python parse_log_match.py
    Loop between ports Gi0/19, Gi0/24, Gi0/16 в VLAN 10

