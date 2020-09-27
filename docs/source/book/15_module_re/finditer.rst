Finditer function
-----------------

Function ``finditer()``: 

* is used to search for all disjoint matches in template
* returns an iterator with Match objects
* finditer() returns iterator even if no match is found

The finditer() function is well suited to handle those commands whose output is displayed by columns. For example: ‘sh ip int br’, ‘sh mac address-table’, etc. In this case it can be applied to the entire output of command.

Example of 'sh ip int br' output:

.. code:: python

    In [8]: sh_ip_int_br = '''
       ...: R1#show ip interface brief
       ...: Interface             IP-Address      OK? Method Status           Protocol
       ...: FastEthernet0/0       15.0.15.1       YES manual up               up
       ...: FastEthernet0/1       10.0.12.1       YES manual up               up
       ...: FastEthernet0/2       10.0.13.1       YES manual up               up
       ...: FastEthernet0/3       unassigned      YES unset  up               up
       ...: Loopback0             10.1.1.1        YES manual up               up
       ...: Loopback100           100.0.0.1       YES manual up               up
       ...: '''

Regular expression for output processing:

.. code:: python

    In [9]: result = re.finditer(r'(\S+) +'
       ...:                      r'([\d.]+) +'
       ...:                      r'\w+ +\w+ +'
       ...:                      r'(up|down|administratively down) +'
       ...:                      r'(up|down)',
       ...:                      sh_ip_int_br)
       ...:

*result* variable contains an iterator:

.. code:: python

    In [12]: result
    Out[12]: <callable_iterator at 0xb583f46c>

Iterator contains Match objects:

.. code:: python

    In [16]: groups = []

    In [18]: for match in result:
        ...:     print(match)
        ...:     groups.append(match.groups())
        ...:
    <_sre.SRE_Match object; span=(103, 171), match='FastEthernet0/0       15.0.15.1       YES manual >
    <_sre.SRE_Match object; span=(172, 240), match='FastEthernet0/1       10.0.12.1       YES manual >
    <_sre.SRE_Match object; span=(241, 309), match='FastEthernet0/2       10.0.13.1       YES manual >
    <_sre.SRE_Match object; span=(379, 447), match='Loopback0             10.1.1.1        YES manual >
    <_sre.SRE_Match object; span=(448, 516), match='Loopback100           100.0.0.1       YES manual >'

Now in *groups* list there are tuples with strings that fallen into groups::

.. code:: python

    In [19]: groups
    Out[19]:
    [('FastEthernet0/0', '15.0.15.1', 'up', 'up'),
     ('FastEthernet0/1', '10.0.12.1', 'up', 'up'),
     ('FastEthernet0/2', '10.0.13.1', 'up', 'up'),
     ('Loopback0', '10.1.1.1', 'up', 'up'),
     ('Loopback100', '100.0.0.1', 'up', 'up')]

A similar result can be obtained by a list generator:

.. code:: python

    In [20]: regex = r'(\S+) +([\d.]+) +\w+ +\w+ +(up|down|administratively down) +(up|down)'

    In [21]: result = [match.groups() for match in re.finditer(regex, sh_ip_int_br)]

    In [22]: result
    Out[22]:
    [('FastEthernet0/0', '15.0.15.1', 'up', 'up'),
     ('FastEthernet0/1', '10.0.12.1', 'up', 'up'),
     ('FastEthernet0/2', '10.0.13.1', 'up', 'up'),
     ('Loopback0', '10.1.1.1', 'up', 'up'),
     ('Loopback100', '100.0.0.1', 'up', 'up')]

Now we will analyze the same log file that was used in *search* and *match* subsections.

In this case it is possible to pass the entire contents of the file (parse_log_finditer.py):

.. code:: python

    import re

    regex = (r'Host \S+ '
             r'in vlan (\d+) '
             r'is flapping between port '
             r'(\S+) and port (\S+)')

    ports = set()

    with open('log.txt') as f:
        for m in re.finditer(regex, f.read()):
            vlan = m.group(1)
            ports.add(m.group(2))
            ports.add(m.group(3))

    print('Loop between ports {} в VLAN {}'.format(', '.join(ports), vlan))

.. warning::

    In real life, a log file can be very large. In that case, it’s better to process it line by line.

Output will be the same:

::

    $ python parse_log_finditer.py
    Loop between ports Gi0/19, Gi0/24, Gi0/16 в VLAN 10

Processing of ‘show cdp neighbors detail’ output
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Finditer() can handle output of ‘sh cdp neighbors detail’ as well as in re.search subsection.

The script is almost identical to the version with re.search (parse_sh_cdp_neighbors_detail_finditer.py file):

.. code:: python

    import re
    from pprint import pprint


    def parse_cdp(filename):
        regex = (r'Device ID: (?P<device>\S+)'
                 r'|IP address: (?P<ip>\S+)'
                 r'|Platform: (?P<platform>\S+ \S+),'
                 r'|Cisco IOS Software, (?P<ios>.+), RELEASE')

        result = {}

        with open(filename) as f:
            match_iter = re.finditer(regex, f.read())
            for match in match_iter:
                if match.lastgroup == 'device':
                    device = match.group(match.lastgroup)
                    result[device] = {}
                elif device:
                    result[device][match.lastgroup] = match.group(match.lastgroup)

        return result

    pprint(parse_cdp('sh_cdp_neighbors_sw1.txt'))

Now matches are searched throughout the file, not in every line separately:

.. code:: python

        with open(filename) as f:
            match_iter = re.finditer(regex, f.read())

Then matches go through the loop:

.. code:: python

        with open(filename) as f:
            match_iter = re.finditer(regex, f.read())
            for match in match_iter:

The rest is the same.

The result will be:

.. code:: python

    $ python parse_sh_cdp_neighbors_detail_finditer.py
    {'R1': {'ios': '3800 Software (C3825-ADVENTERPRISEK9-M), Version 12.4(24)T1',
            'ip': '10.1.1.1',
            'platform': 'Cisco 3825'},
     'R2': {'ios': '2900 Software (C3825-ADVENTERPRISEK9-M), Version 15.2(2)T1',
            'ip': '10.2.2.2',
            'platform': 'Cisco 2911'},
     'SW2': {'ios': 'C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9',
             'ip': '10.1.1.2',
             'platform': 'cisco WS-C2960-8TC-L'}}

Although the result is similar, finditer() has more features, as you can specify not only what should be in searched string but also in strings around it.

For example, you can specify exactly which IP address to take:

::

    Device ID: SW2
    Entry address(es):
      IP address: 10.1.1.2
    Platform: cisco WS-C2960-8TC-L,  Capabilities: Switch IGMP

    ...

    Native VLAN: 1
    Duplex: full
    Management address(es):
      IP address: 10.1.1.2

For example, if you want to take the first IP address you can supplement a regular expression like this:

.. code:: python

    regex = (r'Device ID: (?P<device>\S+)'
             r'|Entry address.*\n +IP address: (?P<ip>\S+)'
             r'|Platform: (?P<platform>\S+ \S+),'
             r'|Cisco IOS Software, (?P<ios>.+), RELEASE')

