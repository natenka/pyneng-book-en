.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 20.1
~~~~~~~~~~~~

Create generate_config function.

Function parameters:

* template - path to the template file (for example, "templates/for.txt")
* data_dict - a dictionary with values to be substituted into the template

The function should return the generated configuration string.

Check the operation of the function on the templates/for.txt template
and data from the data_files/for.yml file.

.. code:: python

    import yaml


    # function call should looks like
    if __name__ == "__main__":
        data_file = "data_files/for.yml"
        template_file = "templates/for.txt"
        with open(data_file) as f:
            data = yaml.safe_load(f)
        print(generate_config(template_file, data))


Task 20.2
~~~~~~~~~~~~

Create template templates/cisco_router_base.txt.

The templates/cisco_router_base.txt template should include the content
of the templates:

* templates/cisco_base.txt
* templates/alias.txt
* templates/eem_int_desc.txt

Template text cannot be copied.

Test the template templates/cisco_router_base.txt using the generate_config
function from task 20.1. Do not copy the code of the generate_config function.

As data, use the information from the data_files/router_info.yml file.

Task 20.3
~~~~~~~~~~~~

Create a template templates/ospf.txt based on the OSPF configuration
in the cisco_ospf.txt file. A configuration example is given to show the syntax.

The template must be created manually by copying parts of the config
into the corresponding template.

What values should be variables:

* process number. Variable name - process
* router-id. Variable name - router_id
* reference-bandwidth. Variable name - ref_bw
* interfaces on which to enable OSPF. The variable name is ospf_intf.
  In place of this variable, a list of dictionaries with the following keys is expected:
   * name - interface name, like Fa0/1, Vlan10, Gi0/0
   * ip - interface IP address, like 10.0.1.1
   * area - zone number
   * passive - whether the interface is passive. Valid values: True or False

For all interfaces in the ospf_intf list, you need to generate the following lines:

::

    network x.x.x.x 0.0.0.0 area x

If the interface is passive, the line must be added for it:

::

    passive-interface x

For interfaces that are not passive, in interface configuration mode,
you need to add the line:

::

    ip ospf hello-interval 1

All commands must be in the appropriate configuration mode.

Check the resulting template templates/ospf.txt, against the data in
the data_files/ospf.yml file, using the generate_config function
from task 20.1. Do not copy the code of the generate_config function.

The result should be a configuration of the following type (the commands
in router ospf mode do not have to be in this order, the main thing is that
they are in the correct config section):

::

    router ospf 10
     router-id 10.0.0.1
     auto-cost reference-bandwidth 20000
     network 10.255.0.1 0.0.0.0 area 0
     network 10.255.1.1 0.0.0.0 area 0
     network 10.255.2.1 0.0.0.0 area 0
     network 10.0.10.1 0.0.0.0 area 2
     network 10.0.20.1 0.0.0.0 area 2
     passive-interface Fa0/0.10
     passive-interface Fa0/0.20
    interface Fa0/1
     ip ospf hello-interval 1
    interface Fa0/1.100
     ip ospf hello-interval 1
    interface Fa0/1.200
     ip ospf hello-interval 1


Задание 20.4
~~~~~~~~~~~~

Create a template templates/add_vlan_to_switch.txt that will be used
if you need to add a VLAN to the switch.

The template must support the following features:

* add VLAN and VLAN name
* adding VLANs as access, on the specified interface
* adding VLANs to the list of allowed, on specified trunks

The template must be created manually by copying parts of the config into
the corresponding template.

If VLAN needs to be added as access, you need to configure the interface
mode and add it to VLAN:

::

    interface Gi0/1
     switchport mode access
     switchport access vlan 5

For trunks, you only need to add VLANs to the allowed list:

::

    interface Gi0/10
     switchport trunk allowed vlan add 5

The variable names should be chosen based on the sample data in
the data_files/add_vlan_to_switch.yaml file.

Check the templates/add_vlan_to_switch.txt template against the data
in data_files/add_vlan_to_switch.yaml using the generate_config function
from task 20.1. Do not copy the code of the generate_config function.


Task 20.5
~~~~~~~~~~~~

Create templates templates/gre_ipsec_vpn_1.txt and templates/gre_ipsec_vpn_2.txt
that generate IPsec over GRE configuration between two routers.

The templates/gre_ipsec_vpn_1.txt template creates the configuration for one
side of the tunnel, and templates gre_ipsec_vpn_2.txt for the other.

Examples of the final configuration that should be generated from templates
in the files: cisco_vpn_1.txt and cisco_vpn_2.txt.

Templates must be created manually by copying parts of the config into
the corresponding templates.

Create a create_vpn_config function that uses these templates to generate
a VPN configuration based on the data in the data dictionary.

Function parameters:

* template1 - the name of the template file that creates the configuration
  for one side of the tunnel
* template2 - the name of the template file that creates the configuration
  for the second side of the tunnel
* data_dict - a dictionary with values to be substituted into templates

The function must return a tuple with two configurations (strings) that are
derived from templates.

Examples of VPN configurations that the create_vpn_config function
should return in the cisco_vpn_1.txt and cisco_vpn_2.txt files.

.. code:: python

    data = {
        'tun_num': 10,
        'wan_ip_1': '192.168.100.1',
        'wan_ip_2': '192.168.100.2',
        'tun_ip_1': '10.0.1.1 255.255.255.252',
        'tun_ip_2': '10.0.1.2 255.255.255.252'
    }

Task 20.5a
~~~~~~~~~~~~~

Create a configure_vpn function that uses the templates from task 20.5
to configure VPN on routers based on the data in the data dictionary.

Function parameters:

* src_device_params - dictionary with connection parameters for device 1
* dst_device_params - dictionary with connection parameters for device 2
* src_template - a template that creates the configuration for side 1
* dst_template - a template that creates the configuration for side 2
* vpn_data_dict - a dictionary with values to be substituted into templates

The function should configure the VPN based on templates and data on each
device using netmiko. The function returns a tuple with the output of commands
from two routers (the output returned by the netmiko send_config_set method).
The first element of the tuple is the output from the first device (string),
the second element of the tuple is the output from the second device.

In this task, the data dictionary does not specify the Tunnel interface
number to use. The number must be determined independently based on information
from the equipment. If the router does not have Tunnel interfaces, take
the number 0, if there is, take the nearest free number, but the same for two routers.

For example, if the src router has the following interfaces: Tunnel1, Tunnel4.
And on the dst router are: Tunnel2, Tunnel3, Tunnel8.
The first free number that is the same for two routers will be 5.
And you will need to configure the Tunnel 5 interface.

For this task, the test verifies that the function works on the first
two devices from the devices.yaml file. And it checks that the output contains
commands for configuring interfaces, but does not check the configured tunnel
numbers and other commands. The tunnels must be configured, but the test has been
simplified so that there are fewer constraints on the task.

.. code:: python

    data = {
        'tun_num': None,
        'wan_ip_1': '192.168.100.1',
        'wan_ip_2': '192.168.100.2',
        'tun_ip_1': '10.0.1.1 255.255.255.252',
        'tun_ip_2': '10.0.1.2 255.255.255.252'
    }

