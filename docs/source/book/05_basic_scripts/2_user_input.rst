User input
-----------------------------

Sometimes it is necessary to get information from user, for example, to request a password.


The ``input()`` function is used to obtain information from user:

.. code:: python

    In [1]: print(input('What is your faivorite routing protocol? '))
    What is your faivorite routing protocol? OSPF
    OSPF

In this case the information is immediately displayed to user, but in addition, the information entered by user can be stored in a variable and can be used later in the script.

.. code:: python

    In [2]: protocol = input('What is your faivorite routing protocol? ')
    What is your faivorite routing protocol? OSPF

    In [3]: print(protocol)
    OSPF

In brackets, a question is usually written that specifies what information to enter.

Request information from script (file access_template_input.py):

.. code:: python

    interface = input('Enter interface type and number: ')
    vlan = input('Enter VLAN number: ')

    access_template = ['switchport mode access',
                       'switchport access vlan {}',
                       'switchport nonegotiate',
                       'spanning-tree portfast',
                       'spanning-tree bpduguard enable']

    print('\n' + '-' * 30)
    print('interface {}'.format(interface))
    print('\n'.join(access_template).format(vlan))

The first two lines request information from user.

The ``print('\n' + '-' * 30)`` line is used to visually separate the information request from the output.

Execution of the script:

::

    $ python access_template_input.py
    Enter interface type and number: Gi0/3
    Enter VLAN number: 55

    ------------------------------
    interface Gi0/3
    switchport mode access
    switchport access vlan 55
    switchport nonegotiate
    spanning-tree portfast
    spanning-tree bpduguard enable

