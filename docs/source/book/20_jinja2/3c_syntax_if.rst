if/elif/else
------------

**if** allows you to add a condition to template. For example, you can use **if** to add parts of template depending on the presence of variables in data dictionary.

**if** construction must also be within  inside ``{% %}``.
End of condition must be explicitly stated:

::

    {% if ospf %}
    router ospf 1
     router-id 10.0.0.{{ id }}
     auto-cost reference-bandwidth 10000
    {% endif %}

Template example templates/if.txt:

::

    hostname {{ name }}

    interface Loopback0
     ip address 10.0.0.{{ id }} 255.255.255.255

    {% for vlan, name in vlans.items() %}
    vlan {{ vlan }}
     name {{ name }}
    {% endfor %}

    {% if ospf %}
    router ospf 1
     router-id 10.0.0.{{ id }}
     auto-cost reference-bandwidth 10000
     {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
     {% endfor %}
    {% endif %}

``if ospf`` expression works the same way as in Python: if variable exists and is not empty, the result is True. If there is no variable or it is empty, the result is False.

That is, in this template the OSPF configuration is generated only if variable *ospf* exists and is not empty.

Configuration will be generated with two data variants.

First with data_files/if.yml that does not contain *ospf* variable:

.. code:: yaml

    id: 3
    name: R3
    vlans:
      10: Marketing
      20: Voice
      30: Management

The result will be:

::

    $ python cfg_gen.py templates/if.txt data_files/if.yml

    hostname R3

    interface Loopback0
     ip address 10.0.0.3 255.255.255.255

    vlan 10
     name Marketing
    vlan 20
     name Voice
    vlan 30
     name Management

Now a similar template but with data_files/if_ospf.yml file:

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

Now the result will be:

::

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

As in Python, Jinja is allowed to make branches in condition.

Template example templates/if_vlans.txt:

::

    {% for intf, params in trunks.items() %}
    interface {{ intf }}
     {% if params.action == 'add' %}
     switchport trunk allowed vlan add {{ params.vlans }}
     {% elif params.action == 'delete' %}
     switchport trunk allowed vlan remove {{ params.vlans }}
     {% else %}
     switchport trunk allowed vlan {{ params.vlans }}
     {% endif %}
    {% endfor %}

Data file data_files/if_vlans.yml:

.. code:: yaml

    trunks:
      Fa0/1:
        action: add
        vlans: 10,20
      Fa0/2:
        action: only
        vlans: 10,30
      Fa0/3:
        action: delete
        vlans: 10

In this example, different commands are generated depending on the value of *action* parameter.

In template you could also use this option to refer to nested dictionaries:

::

    {% for intf in trunks %}
    interface {{ intf }}
     {% if trunks[intf]['action'] == 'add' %}
     switchport trunk allowed vlan add {{ trunks[intf]['vlans'] }}
     {% elif trunks[intf]['action'] == 'delete' %}
     switchport trunk allowed vlan remove {{ trunks[intf]['vlans'] }}
     {% else %}
     switchport trunk allowed vlan {{ trunks[intf]['vlans'] }}
     {% endif %}
    {% endfor %}

This will result in the following configuration:

::

    $ python cfg_gen.py templates/if_vlans.txt data_files/if_vlans.yml
    interface Fa0/1
     switchport trunk allowed vlan add 10,20
    interface Fa0/3
     switchport trunk allowed vlan remove 10
    interface Fa0/2
     switchport trunk allowed vlan 10,30

Using **if** you can also filter which elements of the sequence will be iterated in **for** loop.

Template example templates/if_for.txt with filter in **for** loop:

::

    {% for vlan, name in vlans.items() if vlan > 15 %}
    vlan {{ vlan }}
     name {{ name }}
    {% endfor %}

Data file (data_files/if_for.yml):

.. code:: yaml

    vlans:
      10: Marketing
      20: Voice
      30: Management

The result will be:

::

    $ python cfg_gen.py templates/if_for.txt data_files/if_for.yml
    vlan 20
     name Voice
    vlan 30
     name Management

