include
-------

``Include`` expression allows you to add one template to another.

Variables that are transmitted as data must contain all data for both the
master template and the one that is added through ``include``.

Template templates/vlans.txt:

::

    {% for vlan, name in vlans.items() %}
    vlan {{ vlan }}
     name {{ name }}
    {% endfor %}

Template templates/ospf.txt:

::

    router ospf 1
     auto-cost reference-bandwidth 10000
    {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
    {% endfor %}

Template templates/bgp.txt:

::

    router bgp {{ bgp.local_as }}
    {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
    {% endfor %}
    {% for ebgp in bgp.ebgp_neighbors %}
     neighbor {{ ebgp }} remote-as {{ bgp.ebgp_neighbors[ebgp] }}
    {% endfor %}

Template templates/switch.txt uses created templates ospf and vlans:

::

    {% include 'vlans.txt' %}

    {% include 'ospf.txt' %}

Data file for configuration generation (data_files/switch.yml):

.. code:: json

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

The result of script execution:

::

    $ python cfg_gen.py templates/switch.txt data_files/switch.yml
    vlan 10
     name Marketing
    vlan 20
     name Voice
    vlan 30
     name Management

    router ospf 1
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

The resulting configuration is as if lines from templates ospf.txt and
vlans.txt were in switch.txt template.

Template templates/router.txt:

::

    {% include 'ospf.txt' %}

    {% include 'bgp.txt' %}

    logging {{ log_server }}

In this case, in addition to ``include``, another line in template was added to
show that ``include`` expressions can be mixed with normal template.

Data file (data_files/router.yml):

.. code:: json

    ospf:
      - network: 10.0.1.0 0.0.0.255
        area: 0
      - network: 10.0.2.0 0.0.0.255
        area: 2
      - network: 10.1.1.0 0.0.0.255
        area: 0
    bgp:
      local_as: 100
      loopback: lo100
      ibgp_neighbors:
        - 10.0.0.2
        - 10.0.0.3
      ebgp_neighbors:
        90.1.1.1: 500
        80.1.1.1: 600
    log_server: 10.1.1.1

The result of script execution will be:

::

    $ python cfg_gen.py templates/router.txt data_files/router.yml
    router ospf 1
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

    router bgp 100
     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100
     neighbor 90.1.1.1 remote-as 500
     neighbor 80.1.1.1 remote-as 600

    logging 10.1.1.1

Thanks to ``include``, template templates/ospf.txt is used both in template
templates/switch.txt and in template templates/router.txt, instead of
repeating the same thing twice.
