Flags
-----

When using functions or creating a compiled regular expression you can specify additional flags that affect the behavior of regular expression.

The ``re`` module supports flags (in parentheses - a short version of flag):

* re.ASCII (re.A) 
* re.IGNORECASE (re.I) 
* re.MULTILINE (re.M) 
* re.DOTALL (re.S) 
* re.VERBOSE (re.X) 
* re.LOCALE (re.L) 
* re.DEBUG

In this subsection the re.DOTALL flag is considered. Information about other flags is available in `documentation <https://docs.python.org/3/library/re.html#re.A>`__.

re.DOTALL
^^^^^^^^^

Regular expressions can also be used for multiline string.

For example, from *sh_cdp* string you need to get a device name, platform and IOS:

.. code:: python

    In [2]: sh_cdp = '''
       ...: Device ID: SW2
       ...: Entry address(es):
       ...:   IP address: 10.1.1.2
       ...: Platform: cisco WS-C2960-8TC-L,  Capabilities: Switch IGMP
       ...: Interface: GigabitEthernet1/0/16,  Port ID (outgoing port): GigabitEthernet0/1
       ...: Holdtime : 164 sec
       ...:
       ...: Version :
       ...: Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9, RELEASE SOFTWARE (fc1)
       ...: Technical Support: http://www.cisco.com/techsupport
       ...: Copyright (c) 1986-2014 by Cisco Systems, Inc.
       ...: Compiled Mon 03-Mar-14 22:53 by prod_rel_team
       ...:
       ...: advertisement version: 2
       ...: VTP Management Domain: ''
       ...: Native VLAN: 1
       ...: Duplex: full
       ...: Management address(es):
       ...:   IP address: 10.1.1.2
       ...: '''

Of course, in this case it is possible to divide a string into parts and work with each string separately, but you can get necessary data without splitting.

In this expression, strings with required data are described:

.. code:: python

    In [3]: regex = r'Device ID: (\S+).+Platform: \w+ (\S+),.+Cisco IOS Software.+ Version (\S+),'

In this case, there will be no match because by default a dot means any character other than a new line character:

.. code:: python


    In [4]: print(re.search(regex, sh_cdp))
    None

You can change default behavior by using re.DOTALL flag:

.. code:: python

    In [5]: match = re.search(regex, sh_cdp, re.DOTALL)

    In [6]: match.groups()
    Out[6]: ('SW2', 'WS-C2960-8TC-L', '12.2(55)SE9')

Since new line character is now included, combination ``.+`` captures everything between data.

Now try to use this regular expression to get information about all neighbors from sh_cdp_neighbors_sw1.txt file.

::

    SW1#show cdp neighbors detail
    -------------------------
    Device ID: SW2
    Entry address(es):
      IP address: 10.1.1.2
    Platform: cisco WS-C2960-8TC-L,  Capabilities: Switch IGMP
    Interface: GigabitEthernet1/0/16,  Port ID (outgoing port): GigabitEthernet0/1
    Holdtime : 164 sec

    Version :
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport

    -------------------------
    Device ID: R1
    Entry address(es):
      IP address: 10.1.1.1
    Platform: Cisco 3825,  Capabilities: Router Switch IGMP
    Interface: GigabitEthernet1/0/22,  Port ID (outgoing port): GigabitEthernet0/0
    Holdtime : 156 sec

    Version :
    Cisco IOS Software, 3800 Software (C3825-ADVENTERPRISEK9-M), Version 12.4(24)T1, RELEASE SOFTWARE (fc3)
    Technical Support: http://www.cisco.com/techsupport

    -------------------------
    Device ID: R2
    Entry address(es):
      IP address: 10.2.2.2
    Platform: Cisco 2911,  Capabilities: Router Switch IGMP
    Interface: GigabitEthernet1/0/21,  Port ID (outgoing port): GigabitEthernet0/0
    Holdtime : 156 sec

    Version :
    Cisco IOS Software, 2900 Software (C3825-ADVENTERPRISEK9-M), Version 15.2(2)T1, RELEASE SOFTWARE (fc3)
    Technical Support: http://www.cisco.com/techsupport


Search for all regular expression matches:

.. code:: python

    In [7]: with open('sh_cdp_neighbors_sw1.txt') as f:
       ...:     sh_cdp = f.read()
       ...:

    In [8]: regex = r'Device ID: (\S+).+Platform: \w+ (\S+),.+Cisco IOS Software.+ Version (\S+),'

    In [9]: match = re.finditer(regex, sh_cdp, re.DOTALL)

    In [10]: for m in match:
        ...:     print(m.groups())
        ...:
    ('SW2', '2911', '15.2(2)T1')

At first glance, it seems that instead of three devices there was only one device in output. 
However, if you look at the results the tuple has Device ID from the first neighbor and platform and IOS from the last neighbor.

A short output to ease understanding of result:

::

    Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
    SW2              Gi 1/0/16         171              R S   C2960     Gi 0/1
    R1               Gi 1/0/22         158              R     C3825     Gi 0/0
    R2               Gi 1/0/21         177              R     C2911     Gi 0/0

This is because there is a ``.+`` combination between desired parts of the output.
Without ``re.DOTALL`` flag, such an expression would capture everything before
new line character, but with a flag it captures the longest possible piece of text
because ``+`` is greedy.
As a result, regular expression describes a string from the first Device ID to the
last place where ``Cisco IOS Software.+ Version`` match occurs.

This situation occurs very often when using ``re.DOTALL`` and in order to
correct it remember to disable greedy behavior:

.. code:: python

    In [10]: regex = r'Device ID: (\S+).+?Platform: \w+ (\S+),.+?Cisco IOS Software.+? Version (\S+),'

    In [11]: match = re.finditer(regex, sh_cdp, re.DOTALL)

    In [12]: for m in match:
        ...:     print(m.groups())
        ...:
    ('SW2', 'WS-C2960-8TC-L', '12.2(55)SE9')
    ('R1', '3825', '12.4(24)T1')
    ('R2', '2911', '15.2(2)T1')


