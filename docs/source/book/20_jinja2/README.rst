Getting started with Jinja2
======================

You can install Jinja2 using pip:

.. code:: python

    pip install jinja2

.. note::

    Further, terms Jinja and Jinja2 are used interchangeably.

The main idea of Jinja is to separate data and template. This allows you to use the same template but not the same data.

In the simplest case, template is simply a text file that specifies locations of Jinja variables.

Example of Jinja template:

.. code:: jinja

    hostname {{name}}
    !
    interface Loopback255
     description Management loopback
     ip address 10.255.{{id}}.1 255.255.255.255
    !
    interface GigabitEthernet0/0
     description LAN to {{name}} sw1 {{int}}
     ip address {{ip}} 255.255.255.0
    !
    router ospf 10
     router-id 10.255.{{id}}.1
     auto-cost reference-bandwidth 10000
     network 10.0.0.0 0.255.255.255 area 0

Comments to template:

* In Jinja, variables are written in double curly braces.
* When script is executed, these variables are replaced with desired values.

This template can be used to generate configuration of different devices by substituting other sets of variables.

Example script with file generation based on Jinja template (basic_generator.py file):

.. code:: python

    from jinja2 import Template

    template = Template('''
    hostname {{name}}
    !
    interface Loopback255
     description Management loopback
     ip address 10.255.{{id}}.1 255.255.255.255
    !
    interface GigabitEthernet0/0
     description LAN to {{name}} sw1 {{int}}
     ip address {{ip}} 255.255.255.0
    !
    router ospf 10
     router-id 10.255.{{id}}.1
     auto-cost reference-bandwidth 10000
     network 10.0.0.0 0.255.255.255 area 0
    ''')

    liverpool = {'id':'11', 'name':'Liverpool', 'int':'Gi1/0/17', 'ip':'10.1.1.10'}

    print(template.render(liverpool))

Comments to basic_generator.py file:

* in the first line the Template class is imported from Jinja2 
* creates **template** object to which template is passed
* template uses variables in Jinja syntax
* in *Liverpool* dictionary the keys must be the same as variable names in template 
* values that correspond to the keys - data that will be substituted instead of variables
* the last line renders template using *liverpool* dictionary, that is, sets values in variables.

If you run basic_generator.py script, the output is:

::

    $ python basic_generator.py

    hostname Liverpool
    !
    interface Loopback255
     description Management loopback
     ip address 10.255.11.1 255.255.255.255
    !
    interface GigabitEthernet0/0
     description LAN to Liverpool sw1 Gi1/0/17
     ip address 10.1.1.10 255.255.255.0
    !
    router ospf 10
     router-id 10.255.11.1
     auto-cost reference-bandwidth 10000
     network 10.0.0.0 0.255.255.255 area 0

