.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 20.1
~~~~~~~~~~~~

Create generate_config() function.

Function parameters:

* template - path to template file (for example, "templates/for.txt")
* data_dict - dictionary with values to set in template

Function should return a string with configuration that has been generated.

Check function with templates/for.txt template and data from data_files/for.yml.

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

Create templates/cisco_router_base.txt template. It should include template content of:

* templates/cisco_base.txt
* templates/alias.txt
* templates/eem_int_desc.txt

You cannot copy template text.

Check templates/cisco_router_base.txt template using generate_config() functon from task 20.1. Do not copy generate_config() function code.

As data, use information from data_files/router_info.yml file

Task 20.3
~~~~~~~~~~~~

Create templates/ospf.txt template based on OSPF configuration in cisco_ospf.txt file. Configuration example is given to show syntax.

Create template manually by copying parts of configuration into corresponding template.

Which values should be variables:

* process number. Variable name - process
* router-id. Variable name - router_id
* reference-bandwidth. Variable name - ref_bw
* interfaces to enable OSPF. Variable name - ospf_intf. This variable expects list of dictionaries with such keys:

  * name - interface name like Fa0/1, Vlan10, Gi0/0
  * ip - IP address of interface like 10.0.1.1
  * area - area number
  * passive - whether interface is passive. Valid values: True or False

For all interfaces in *ospf_intf* list you should generate lines::

::   network x.x.x.x 0.0.0.0 area x

If interface is passive, this line should be added:

::   passive-interface x

For interfaces that are not passive, in interface configuration mode you should add a line:

::   ip ospf hello-interval 1

All commands must be in appropriate modes.

Check resulting templates/ospf.txt template with data in data_files/ospf.yml file using generate_config() function from task 20.1. Do not copy generate_config() function code.


The result should be a configuration of this kind (commands inside *router ospf* mode don’t have to be in such order, more important that they are in the right mode):

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

Create templates/add_vlan_to_switch.txt template that will be used if it's necessary to add VLAN to switch.

Template should support these features:

* add VLAN and VLAN name
* add VLAN as access on specified interface
* add VLAN to list of allowed vlans on trunks

If you want to add VLAN as access, you need to configure interface mode and add VLAN to it:

::

    interface Gi0/1
     switchport mode access
     switchport access vlan 5

For trunks, only add VLAN to the list of allowed vlans:

::

    interface Gi0/10
     switchport trunk allowed vlan add 5

Variable names should be selected from data example in data_files/add_vlan_to_switch.yaml.

Check templates/add_vlan_to_switch.txt with data in data_files/add_vlan_to_switch.yaml file  using generat_config() function from task 20.1. Do not copy generate_config() function code.


Task 20.5
~~~~~~~~~~~~

Create templates/gre_ipsec_vpn_1.txt and templates/gre_ipsec_vpn_2.txt templates that generate IPsec over GRE configuration between two routers.

Template templates/gre_ipsec_vpn_1.txt creates a configuration for one side of tunnel and templates/gre_ipsec_vpn_2.txt for other side.

Examples of resulting configuration that should be created based on templates in files: cisco_vpn_1.txt and cisco_vpn_2.txt.

Create create_vpn_config() function that uses these templates to generate VPN configuration based on data in *data* dictionary.

Function parameters:

* template1 - file name with template that creates configuration for one tunnel side
* template2 - file name with template that creates configuration for second tunnel side
* data_dict - dictionary with values to set in templates

Function should return a tuple with two configurations (strings) that are derived from templates.

In cisco_vpn_1.txt and cisco_vpn_2.txt you will find examples of VPN configurations that should return create_vpn_config() function.

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

Create configure_vpn() function that uses templates from task 20.5 to configure VPN on routers based on data in *data* dictionary.

Function parameters:

* src_device_params - dictionary with device connection parameters
* dst_device_params - dictionary with device connection parameters
* src_template - name of file with template that creates a configuration for one tunnel side
* dst_template - name of file with template that creates a configuration for second tunnel side
* vpn_data_dict - dictionary with values to set in templates

Function should configure VPN based on templates and data on each device using netmiko. Function returns output with a set of commands from two routers (output that returns netmiko send_config_set() method).

However, *data* dictionary does not specify Tunnel interface number to be used. Number has to be determined independently based on information from equipment. If there are no Tunnel interfaces on router, take number 0. If there are some interfaces, take the nearest available number but it should be the same for two routers.

For example, *src* router has such interfaces as Tunnel1, Tunnel4. On *dest* router: Tunnel2, Tunnel3, Tunnel8. The first available number for two routers will be 9. And you will need to configure Tunnel9 interface.

.. note::

    To complicate task you can make that number 5 is taken instead of 9.

There’s no test for this task!

.. code:: python

    data = {
        'tun_num': None,
        'wan_ip_1': '192.168.100.1',
        'wan_ip_2': '192.168.100.2',
        'tun_ip_1': '10.0.1.1 255.255.255.252',
        'tun_ip_2': '10.0.1.2 255.255.255.252'
    }

