Usage example of Jinja2
---------------------------

In this example, logic is divided into 3 different files (all files are in 1_example directory):

* router_template.py - template 
* routers_info.yml - this file contains a list of dictionaries (in YAML format) with information about routers for which you need to generate a configuration file
* router_config_generator.py - this script imports a template file and reads information from a YAML file and then generates router configuration files

File router_template.py

.. code:: python

    # -*- coding: utf-8 -*-
    from jinja2 import Template

    template_r1 = Template('''
    hostname {{name}}
    !
    interface Loopback10
     description MPLS loopback
     ip address 10.10.{{id}}.1 255.255.255.255
     !
    interface GigabitEthernet0/0
     description WAN to {{name}} sw1 G0/1
    !
    interface GigabitEthernet0/0.1{{id}}1
     description MPLS to {{to_name}}
     encapsulation dot1Q 1{{id}}1
     ip address 10.{{id}}.1.2 255.255.255.252
     ip ospf network point-to-point
     ip ospf hello-interval 1
     ip ospf cost 10
    !
    interface GigabitEthernet0/1
     description LAN {{name}} to sw1 G0/2 !
    interface GigabitEthernet0/1.{{IT}}
     description PW IT {{name}} - {{to_name}}
     encapsulation dot1Q {{IT}}
     xconnect 10.10.{{to_id}}.1 {{id}}11 encapsulation mpls
     backup peer 10.10.{{to_id}}.2 {{id}}21
      backup delay 1 1
    !
    interface GigabitEthernet0/1.{{BS}}
     description PW BS {{name}} - {{to_name}}
     encapsulation dot1Q {{BS}}
     xconnect 10.10.{{to_id}}.1 {{to_id}}{{id}}11 encapsulation mpls
      backup peer 10.10.{{to_id}}.2 {{to_id}}{{id}}21
      backup delay 1 1
    !
    router ospf 10
     router-id 10.10.{{id}}.1
     auto-cost reference-bandwidth 10000
     network 10.0.0.0 0.255.255.255 area 0
     !
    ''')

File routers_info.yml

.. code:: yaml

    - id: 11
      name: Liverpool
      to_name: LONDON
      IT: 791
      BS: 1550
      to_id: 1

    - id: 12
      name: Bristol
      to_name: LONDON
      IT: 793
      BS: 1510
      to_id: 1

    - id: 14
      name: Coventry
      to_name: Manchester
      IT: 892
      BS: 1650
      to_id: 2

File router_config_generator.py

.. code:: python

    # -*- coding: utf-8 -*-
    import yaml
    from router_template import template_r1

    with open('routers_info.yml') as f:
        routers = yaml.safe_load(f)

    for router in routers:
        r1_conf = router['name']+'_r1.txt'
        with open(r1_conf,'w') as f:
            f.write(template_r1.render(router))

File router_config_generator.py: 

* imports template template_r1 
* from routers_info.yml file the parameter list is read to *routers* variable

Then objects (dictionaries) in *routers* list are iterated in the loop:

* name of file into which the final configuration is written consists of *name* field in dictionary and r1.txt string. For example, Liverpool_r1.txt
* file with this name opens in write mode
* file records the result of template rendering using current dictionary 
* construction *with* closes the file
* control returns to the beginning of loop (until all dictionaries are iterated)

Run file router_config_generator.py:

::

    $ python router_config_generator.py

The result is three configuration files:

::

    hostname Liverpool
    !
    interface Loopback10
     description MPLS loopback
     ip address 10.10.11.1 255.255.255.255
    !
    interface GigabitEthernet0/0
     description WAN to Liverpool sw1 G0/1
    !
    interface GigabitEthernet0/0.1111
     description MPLS to LONDON
     encapsulation dot1Q 1111
     ip address 10.11.1.2 255.255.255.252
     ip ospf network point-to-point
     ip ospf hello-interval 1
     ip ospf cost 10
    !
    interface GigabitEthernet0/1
     description LAN Liverpool to sw1 G0/2
    !
    interface GigabitEthernet0/1.791
     description PW IT Liverpool - LONDON
     encapsulation dot1Q 791
     xconnect 10.10.1.1 1111 encapsulation mpls
      backup peer 10.10.1.2 1121
      backup delay 1 1
    !
    interface GigabitEthernet0/1.1550
     description PW BS Liverpool - LONDON
     encapsulation dot1Q 1550
     xconnect 10.10.1.1 11111 encapsulation mpls
      backup peer 10.10.1.2 11121
      backup delay 1 1
    !
    router ospf 10
     router-id 10.10.11.1
     auto-cost reference-bandwidth 10000
     network 10.0.0.0 0.255.255.255 area 0
    !

