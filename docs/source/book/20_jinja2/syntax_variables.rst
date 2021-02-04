Variables
----------

Variables in template are given in double curly braces:

::

    hostname {{ name }}

    interface Loopback0
     ip address 10.0.0.{{ id }} 255.255.255.255

Variable values are set based on dictionary that is passed to template.

Variable that is passed on in a dictionary may not only be a number or a string, but also for example, a list or a dictionary. Inside template, you can refer to the item by number or key.

Template example templates/variables.txt with usage of different variable variants:

::

    hostname {{ name }}

    interface Loopback0
     ip address 10.0.0.{{ id }} 255.255.255.255

    vlan {{ vlans[0] }}

    router ospf 1
     router-id 10.0.0.{{ id }}
     auto-cost reference-bandwidth 10000
     network {{ ospf.network }} area {{ ospf['area'] }}

And corresponding data_files/vars.yml file with variables:

::

    id: 3
    name: R3
    vlans:
      - 10
      - 20
      - 30
    ospf:
      network: 10.0.1.0 0.0.0.255
      area: 0

Note the use of *vlans* variable in template: since *vlans* variable is a list, you can specify which item from list we need

If a dictionary is passed (as in case of  *ospf* variable), you can refer to dictionary objects inside template using one of the variants:  ``ospf.network or ospf['network']``

The result will be:

::

    $ python cfg_gen.py templates/variables.txt data_files/vars.yml
    hostname R3

    interface Loopback0
     ip address 10.0.0.3 255.255.255.255

    vlan 10

    router ospf 1
     router-id 10.0.0.3
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0

