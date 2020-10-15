Nested for
~~~~~~~~~~~~~

Loops **for** can be nested in each other.

In this example, *commands* is a list of commands to execute on each interface in the *fast_int* list:

.. code:: python

    In [7]: commands = ['switchport mode access', 'spanning-tree portfast', 'spanning-tree bpduguard enable']
    In [8]: fast_int = ['0/1','0/3','0/4','0/7','0/9','0/10','0/11']

    In [9]: for intf in fast_int:
       ...:     print('interface FastEthernet {}'.format(intf))
       ...:     for command in commands:
       ...:         print(' {}'.format(command))
       ...:
    interface FastEthernet 0/1
     switchport mode access
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet 0/3
     switchport mode access
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet 0/4
     switchport mode access
     spanning-tree portfast
     spanning-tree bpduguard enable
    ...

The first **for** loop passes through interfaces in the *fast_int* list and the second through commands in *commands* list.
