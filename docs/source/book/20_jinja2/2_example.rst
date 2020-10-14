Example of using Jinja with correct use of software interface
------------------------------------------------------------------------------

To deal with Jinja2, it is better to use previous examples. This section describes the correct use of Jinja. In this version, data, template and script that generates the resulting information are separated.

.. note::

    Term "software interface" refers to the way Jinja works with input data and a template for generating output files.
    
Modified example of previous script, template and data file (all files are in 2_example directory):

Template templates/router_template.txt is a plain text file:

::

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

Data file routers_info.yml

::

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

Script to generate configurations router_config_generator_ver2.py

.. code:: python

    # -*- coding: utf-8 -*-
    from jinja2 import Environment, FileSystemLoader
    import yaml

    env = Environment(loader=FileSystemLoader('templates'))
    template = env.get_template('router_template.txt')

    with open('routers_info.yml') as f:
        routers = yaml.safe_load(f)

    for router in routers:
        r1_conf = router['name']+'_r1.txt'
        with open(r1_conf,'w') as f:
            f.write(template.render(router))

File router_config_generator.py imports from jinja2 module:

* **FileSystemLoader** - a loader that allows working with a file system

  * path to directory where templates are located is specified here
  * in this case template is in *template* directory
  
* **Environment** - a class for describing environment parameters. In this case only loader is specified, but you can specify how to process a template

Note that template is now in **templates** directory.

