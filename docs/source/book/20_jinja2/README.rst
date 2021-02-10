Getting started with Jinja2
======================

You can install Jinja2 using pip:

.. code:: python

    pip install jinja2

.. note::

    Further, terms Jinja and Jinja2 are used interchangeably.

The main idea of Jinja is to separate data and template. This allows you to use
the same template but not the same data.
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

This template can be used to generate configuration of different devices by
substituting other sets of variables.

