User input
-----------------------------

Sometimes it is necessary to get information from user. For example, request a password.
The ``input`` function is used to get information from the user:

.. code:: python

    In [1]: print(input('What is your faivorite routing protocol? '))
    What is your faivorite routing protocol? OSPF
    OSPF

In this case, information is immediately displayed to user, but in addition, information entered by user can be stored in a variable and can be used later in script.

.. code:: python

    In [2]: protocol = input('What is your faivorite routing protocol? ')
    What is your faivorite routing protocol? OSPF

    In [3]: print(protocol)
    OSPF

In parentheses, a question is usually written that specifies what information needs to be entered.
A script that asks the user for information using input (file access_template_input.py):

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

First two lines request information from user.
Line ``print('\n' + '-' * 30)`` is used to visually separate information request from the output.

Script output:

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

