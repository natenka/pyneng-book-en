Parsing the output of 'show ip dhcp snooping' command using named groups
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Consider another example of using named groups. In this example, the task is
to get from the output of 'show ip dhcp snooping binding' the fields: MAC address, IP address, VLAN and interface.

File dhcp_snooping.txt contains the output of command 'show ip dhcp snooping binding':

::

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:09:BB:3D:D6:58   10.1.10.2        86250       dhcp-snooping   10    FastEthernet0/1
    00:04:A3:3E:5B:69   10.1.5.2         63951       dhcp-snooping   5     FastEthernet0/10
    00:05:B3:7E:9B:60   10.1.5.4         63253       dhcp-snooping   5     FastEthernet0/9
    00:09:BC:3F:A6:50   10.1.10.6        76260       dhcp-snooping   10    FastEthernet0/3
    Total number of bindings: 4

Let's start with one string:

.. code:: python

    In [1]: line = '00:09:BB:3D:D6:58  10.1.10.2 86250   dhcp-snooping   10  FastEthernet0/1'

In regex terms, named groups are used for those parts of the output that need to be remembered:

.. code:: python

    In [2]: match = re.search('(?P<mac>\S+) +(?P<ip>\S+) +\d+ +\S+ +(?P<vlan>\d+) +(?P<port>\S+)', line)

Comments on regex:

-  ``(?P<mac>\S+) +`` - group with name 'mac' matches any characters except whitespace characters. regex describes a sequence of any characters before space
-  ``(?P<ip>\S+) +`` - the same here: a sequence of any non-whitespace characters up to space. Group name - 'ip'
-  ``\d+ +`` - numerical sequence (one or more digits) followed by one or more spaces. *Lease* value gets here
-  ``\S+ +``- sequence of any characters other than whitespace. This matches *Type* (in this case all of them 'dhcp-snooping')
-  ``(?P<vlan>\d+) +`` - named group 'vlan'. Only numerical sequences with one or more characters are included here
-  ``(?P<port>.\S+)`` - named group 'port'. All characters except whitespace are included here

As a result, ``groupdict`` method will return such a dictionary:

.. code:: python

    In [3]: match.groupdict()
    Out[3]: 
    {'int': 'FastEthernet0/1',
     'ip': '10.1.10.2',
     'mac': '00:09:BB:3D:D6:58',
     'vlan': '10'}

Since regex has worked well, you can create a script. In script all
lines of dhcp\_snooping.txt file are iterated and information about devices is
displayed on the standard output stream (parse_dhcp_snooping.py):

.. code:: python

    import re

    #'00:09:BB:3D:D6:58   10.1.10.2        86250       dhcp-snooping   10    FastEthernet0/1'
    regex = re.compile('(?P<mac>\S+) +(?P<ip>\S+) +\d+ +\S+ +(?P<vlan>\d+) +(?P<port>\S+)')
    result = []

    with open('dhcp_snooping.txt') as data:
        for line in data:
            match = regex.search(line)
            if match:
                result.append(match.groupdict())

    print(f'{len(result)} devices connected to switch')

    for num, comp in enumerate(result, 1):
        print(f'Parameters of device {num}:')
        for key in comp:
            print(f'{key:10}: {comp[key]:10}')

Script output:

::

    $ python parse_dhcp_snooping.py
    4 devices connected to switch
    Parameters of device 1:
        int:    FastEthernet0/1
        ip:    10.1.10.2
        mac:    00:09:BB:3D:D6:58
        vlan:    10
    Parameters of device 2:
        int:    FastEthernet0/10
        ip:    10.1.5.2
        mac:    00:04:A3:3E:5B:69
        vlan:    5
    Parameters of device 3:
        int:    FastEthernet0/9
        ip:    10.1.5.4
        mac:    00:05:B3:7E:9B:60
        vlan:    5
    Parameters of device 4:
        int:    FastEthernet0/3
        ip:    10.1.10.6
        mac:    00:09:BC:3F:A6:50
        vlan:    10

