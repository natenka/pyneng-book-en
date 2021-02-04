Loop for
--------

Loop ``for`` allows you to walk through sequence of elements.

Loop ``for`` must be written inside ``{% %}``.
Furthermore, the end of the loop must be explicitly indicated:

::

    {% for vlan in vlans %}
      vlan {{ vlan }}
    {% endfor %}

Template example templates/for.txt using a loop:

::

    hostname {{ name }}

    interface Loopback0
     ip address 10.0.0.{{ id }} 255.255.255.255

    {% for vlan, name in vlans.items() %}
    vlan {{ vlan }}
     name {{ name }}
    {% endfor %}

    router ospf 1
     router-id 10.0.0.{{ id }}
     auto-cost reference-bandwidth 10000
     {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
     {% endfor %}

File data_files/for.yml with variables:

.. code:: yaml

    id: 3
    name: R3
    vlans:
      10: Marketing
      20: Voice
      30: Management
    ospf:
      - network: 10.0.1.0 0.0.0.255
        area: 0
      - network: 10.0.2.0 0.0.0.255
        area: 2
      - network: 10.1.1.0 0.0.0.255
        area: 0

In ``for``, it is possible to go through both the list elements (for example,
ospf list) and the dictionary (vlans dictionary). And similarly, through any sequence.

The result will be:

::

    $ python cfg_gen.py templates/for.txt data_files/for.yml
    hostname R3

    interface Loopback0
     ip address 10.0.0.3 255.255.255.255

    vlan 10
     name Marketing
    vlan 20
     name Voice
    vlan 30
     name Management

    router ospf 1
     router-id 10.0.0.3
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

