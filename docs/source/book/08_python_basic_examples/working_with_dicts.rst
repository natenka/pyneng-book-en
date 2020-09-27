Working with dictionary
-------------------

When processing output of commands or configuration, often it will be necessary to write the summary data to the dictionary.

It is not always obvious how to handle the output of commands and how to deal with the output in general. This subsection discusses several examples with increasing complexity.

Parsing of output with columns
~~~~~~~~~~~~~~~~~~~~~~~

This example will deal with the output of *sh ip int br* command. From the output of command we need to get the interface name - IP address. So the interface name is the dictionary key and the IP address is the value. At the same time, the match must be made only for those interfaces with the IP address assigned.

An example of *sh ip int br* output (sh_ip_int_br.txt file):

::

    R1#show ip interface brief
    Interface                  IP-Address      OK? Method Status                Protocol
    FastEthernet0/0            15.0.15.1       YES manual up                    up
    FastEthernet0/1            10.0.12.1       YES manual up                    up
    FastEthernet0/2            10.0.13.1       YES manual up                    up
    FastEthernet0/3            unassigned      YES unset  up                    down
    Loopback0                  10.1.1.1        YES manual up                    up
    Loopback100                100.0.0.1       YES manual up                    up

Working_with_dict_example_1.py file:

.. code:: python

    result = {}

    with open('sh_ip_int_br.txt') as f:
        for line in f:
            line = line.split()
            if line and line[1][0].isdigit():
                interface, address, *other = line
                result[interface] = address

    print(result)

The command *sh ip int br* displays the output with columns. So the desired fields are in the same line. The script processes the output line by line and divides each line using split() method.

The resulting list contains output columns. Because we need only interfaces on which the IP address is configured, the first character of the second column is checked: if the first character is a number the address is assigned to the interface and the string has to be processed.

In ``interface, address, *other = line`` - the variables are unpacked. The *interface* variable will have the interface name, the *address* will have the IP address and *other* - all other fields.

Since each line has a key-value pair, they are assigned to the dictionary: ``result[interface] = address``.

The result of the script execution will be a dictionary (here it is split into key-value pairs for convenience, in the real script output the dictionary will be displayed in one line):

.. code:: python

    {'FastEthernet0/0': '15.0.15.1',
     'FastEthernet0/1': '10.0.12.1',
     'FastEthernet0/2': '10.0.13.1',
     'Loopback0': '10.1.1.1',
     'Loopback100': '100.0.0.1'}

Getting key and value from different output lines
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Very often the output of commands looks like the key and the value are in different lines. And you have to figure out how to process the output to get the right match.

For example, from the output of the *sh ip int br* command you need to get the match *interface name – MTU* (sh_ip_interface.txt file):

::

    Ethernet0/0 is up, line protocol is up
      Internet address is 192.168.100.1/24
      Broadcast address is 255.255.255.255
      Address determined by non-volatile memory
      MTU is 1500 bytes
      Helper address is not set
      ...
    Ethernet0/1 is up, line protocol is up
      Internet address is 192.168.200.1/24
      Broadcast address is 255.255.255.255
      Address determined by non-volatile memory
      MTU is 1500 bytes
      Helper address is not set
      ...
    Ethernet0/2 is up, line protocol is up
      Internet address is 19.1.1.1/24
      Broadcast address is 255.255.255.255
      Address determined by non-volatile memory
      MTU is 1500 bytes
      Helper address is not set
      ...

The interface name is in ``Ethernet0/0 is up, line protocol is up`` line and MTU in the ``MTU is 1500 bytes`` line.

For example, try to remember the interface each time and print its value when MTU parameter is detected, together with MTU value:

.. code:: python

    In [2]: with open('sh_ip_interface.txt') as f:
       ...:     for line in f:
       ...:         if 'line protocol' in line:
       ...:             interface = line.split()[0]
       ...:         elif 'MTU is' in line:
       ...:             mtu = line.split()[-2]
       ...:             print('{:15}{}'.format(interface, mtu))
       ...:
    Ethernet0/0    1500
    Ethernet0/1    1500
    Ethernet0/2    1500
    Ethernet0/3    1500
    Loopback0      1514

The command output is organized in such a way that there is always a line with interface first and then a line with MTU after several lines. If you remember the name of the interface every time it appears and at the time when line meets with MTU, the last memorized interface is the one which matches this MTU.

Now, if you want to create a dictionary that matches *interface – MTU*, it’s enough to write the values when the MTU was found.

Working_with_dict_example_2.py file:

.. code:: python

    result = {}

    with open('sh_ip_interface.txt') as f:
        for line in f:
            if 'line protocol' in line:
                interface = line.split()[0]
            elif 'MTU is' in line:
                mtu = line.split()[-2]
                result[interface] = mtu

    print(result)

The result of the script execution will be a dictionary (here it is split into key-value pairs for convenience, in the real script output the dictionary will be displayed in one line):

.. code:: python

    {'Ethernet0/0': '1500',
     'Ethernet0/1': '1500',
     'Ethernet0/2': '1500',
     'Ethernet0/3': '1500',
     'Loopback0': '1514'}

This technique will be quite often useful because command output is generally organized in a very similar way.

Nested dictionary
~~~~~~~~~~~~~~~~~

If you want to get several parameters from the output, it is very convenient to use a dictionary with a nested dictionary.

For example, from output ```sh ip interface``` you need to get two parameters: IP address and MTU. First, output of the information:

::

    Ethernet0/0 is up, line protocol is up
      Internet address is 192.168.100.1/24
      Broadcast address is 255.255.255.255
      Address determined by non-volatile memory
      MTU is 1500 bytes
      Helper address is not set
      ...
    Ethernet0/1 is up, line protocol is up
      Internet address is 192.168.200.1/24
      Broadcast address is 255.255.255.255
      Address determined by non-volatile memory
      MTU is 1500 bytes
      Helper address is not set
      ...
    Ethernet0/2 is up, line protocol is up
      Internet address is 19.1.1.1/24
      Broadcast address is 255.255.255.255
      Address determined by non-volatile memory
      MTU is 1500 bytes
      Helper address is not set
      ...

In the first step, each value is stored in a variable and then all three values are displayed. The values are displayed when a string has MTU because it is the last string:

.. code:: python

    In [2]: with open('sh_ip_interface.txt') as f:
       ...:     for line in f:
       ...:         if 'line protocol' in line:
       ...:             interface = line.split()[0]
       ...:         elif 'Internet address' in line:
       ...:             ip_address = line.split()[-1]
       ...:         elif 'MTU' in line:
       ...:             mtu = line.split()[-2]
       ...:             print('{:15}{:17}{}'.format(interface, ip_address, mtu))
       ...:
    Ethernet0/0    192.168.100.1/24 1500
    Ethernet0/1    192.168.200.1/24 1500
    Ethernet0/2    19.1.1.1/24      1500
    Ethernet0/3    192.168.230.1/24 1500
    Loopback0      4.4.4.4/32       1514

It uses the same technique as in the previous example but adds another nested dictionary:

.. code:: python

    result = {}

    with open('sh_ip_interface.txt') as f:
        for line in f:
            if 'line protocol' in line:
                interface = line.split()[0]
                result[interface] = {}
            elif 'Internet address' in line:
                ip_address = line.split()[-1]
                result[interface]['ip'] = ip_address
            elif 'MTU' in line:
                mtu = line.split()[-2]
                result[interface]['mtu'] = mtu

    print(result)

Each time an interface is detected, the dictionary ```result``` creates a key with the name of the interface that corresponds to an empty dictionary. This blank is used so that at the time when the IP address or MTU is detected, the parameter can be written into the nested dictionary of the corresponding interface.

The result of the script execution will be a dictionary (here it is split into key-value pairs for convenience, in the real script output the dictionary will be displayed in one line):

.. code:: python

    {'Ethernet0/0': {'ip': '192.168.100.1/24', 'mtu': '1500'},
     'Ethernet0/1': {'ip': '192.168.200.1/24', 'mtu': '1500'},
     'Ethernet0/2': {'ip': '19.1.1.1/24', 'mtu': '1500'},
     'Ethernet0/3': {'ip': '192.168.230.1/24', 'mtu': '1500'},
     'Loopback0': {'ip': '4.4.4.4/32', 'mtu': '1514'}}

Output with empty values
~~~~~~~~~~~~~~~~~~~~~~~~~~

Sometimes, sections with empty values will be found in the output. For example, in the case of output ```sh ip interface```, interfaces may look like:
::

    Ethernet0/1 is up, line protocol is up
      Internet protocol processing disabled
    Ethernet0/2 is administratively down, line protocol is down
      Internet protocol processing disabled
    Ethernet0/3 is administratively down, line protocol is down
      Internet protocol processing disabled

Consequently, there is no MTU or IP address.

And if you execute the previous script for a file with such interfaces, the result is this (output for the file sh_ip_interface2.txt):

.. code:: python

    {'Ethernet0/0': {'ip': '192.168.100.2/24', 'mtu': '1500'},
     'Ethernet0/1': {},
     'Ethernet0/2': {},
     'Ethernet0/3': {},
     'Loopback0': {'ip': '2.2.2.2/32', 'mtu': '1514'}}

If you need to add interfaces to the dictionary only when an IP address is assigned to the interface, you need to move the creation of the key with interface name to the moment when the line with IP address is detected (working_with_dict_example_4.py file):

.. code:: python

    result = {}

    with open('sh_ip_interface2.txt') as f:
        for line in f:
            if 'line protocol' in line:
                interface = line.split()[0]
            elif 'Internet address' in line:
                ip_address = line.split()[-1]
                result[interface] = {}
                result[interface]['ip'] = ip_address
            elif 'MTU' in line:
                mtu = line.split()[-2]
                result[interface]['mtu'] = mtu

    print(result)

In this case, the result will be a dictionary:

.. code:: python

    {'Ethernet0/0': {'ip': '192.168.100.2/24', 'mtu': '1500'},
     'Loopback0': {'ip': '2.2.2.2/32', 'mtu': '1514'}}

