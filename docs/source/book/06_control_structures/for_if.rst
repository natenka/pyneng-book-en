Combination for and if
~~~~~~~~~~~~~~~~~~~

Consider example of combining **for** and **if**.

Generate_access_port_config.py file:

.. code-block:: python
   :linenos:

    access_template = ['switchport mode access',
                       'switchport access vlan',
                       'spanning-tree portfast',
                       'spanning-tree bpduguard enable']

    fast_int = {'access': { '0/12':10,
                            '0/14':11,
                            '0/16':17,
                            '0/17':150}}

    for intf, vlan in fast_int['access'].items():
        print('interface FastEthernet' + intf)
        for command in access_template:
            if command.endswith('access vlan'):
                print(' {} {}'.format(command, vlan))
            else:
                print(' {}'.format(command))

Comments to the code:

* The first **for** loop iterates keys and values in nested fast\_int['access'] dictionary
* At this moment of loop the current key is stored in *intf* variable
* At this moment of loop the current value is stored in *vlan* variable
* String “interface Fastethernet” is displayed with interface number added
* The second loop **for** iterates commands from *access_template* list
* Since *switchport access to vlan* command requires a VLAN number:

  * within second loop **for** commands are checked
  * if command ends with “access vlan”
  
    * command is displayed and VLAN number is added to it

  * in all other cases, command is simply displayed


Result of script execution:

::

    $ python generate_access_port_config.py
    interface FastEthernet0/12
     switchport mode access
     switchport access vlan 10
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet0/14
     switchport mode access
     switchport access vlan 11
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet0/16
     switchport mode access
     switchport access vlan 17
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet0/17
     switchport mode access
     switchport access vlan 150
     spanning-tree portfast
     spanning-tree bpduguard enable

