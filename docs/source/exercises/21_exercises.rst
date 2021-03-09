.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 21.1
~~~~~~~~~~~~

Create parse_command_output function. Function parameters:

* template - name of the file containing the TextFSM template.
  For example templates/sh_ip_int_br.template
* command_output - output the corresponding show command (string)

The function should return a list:

* the first element is a list with column names
* the rest of the items are lists, which contain the results
  of processing the output of the show command

Check the operation of the function on the output of the sh ip int br command
from the equipment and on the templates/sh_ip_int_br.template template.

.. code:: python

    from netmiko import ConnectHandler


    # this is how a function call should look
    if __name__ == "__main__":
        r1_params = {
            "device_type": "cisco_ios",
            "host": "192.168.100.1",
            "username": "cisco",
            "password": "cisco",
            "secret": "cisco",
        }
        with ConnectHandler(**r1_params) as r1:
            r1.enable()
            output = r1.send_command("sh ip int br")
        result = parse_command_output("templates/sh_ip_int_br.template", output)
        print(result)


Task 21.1a
~~~~~~~~~~~~~

Create parse_output_to_dict function.

Function parameters:

* template is the name of the file containing the TextFSM template.
  For example templates/sh_ip_int_br.template
* command_output - output of the corresponding show command (string)

The function should return a list of dictionaries:

* keys - names of variables in the TextFSM template
* values - parts of the output that correspond to variables

Check the operation of the function on the output of the command
output/sh_ip_int_br.txt and the template templates/sh_ip_int_br.template.

Task 21.2
~~~~~~~~~~~~

Create a TextFSM template to parse the output of the sh ip dhcp snooping binding
command and write it to templates/sh_ip_dhcp_snooping.template

The command output is located in the file output/sh_ip_dhcp_snooping.txt.

The template should process and return the values of such columns:

* mac - 00:04:A3:3E:5B:69
* ip - 10.1.10.6
* vlan - 10
* intf - FastEthernet0/10

Check the work of the template using the parse_command_output function
from task 21.1.

Task 21.3
~~~~~~~~~~

Create function parse_command_dynamic.

Function parameters:

* command_output - command output (string)
* attributes_dict - an attribute dict containing the following key-value pairs:

  * 'Command': command
  * 'Vendor': vendor

* index_file is the name of the file where the correspondence between commands
  and templates is stored. The default is "index"
* templ_path - directory where templates are stored. The default is "templates"

The function should return a list of dicts with the results
of parsing the command output (as in task 21.1a):

* keys - names of variables in the TextFSM template
* values - parts of the output that correspond to variables

Check the function on the output of the sh ip int br command.

Task 21.4
~~~~~~~~~~~~

Create function send_and_parse_show_command.

Function parameters:

* device_dict - a dict with connectin parameters for one device
* command - the command to be executed
* templates_path - path to the directory with TextFSM templates
* index - file index name, default value "index"

The function should connect to one device, send a show command using netmiko,
and then parse the command output using TextFSM.

The function should return a list of dictionaries with the results
of parsing the command output (as in task 21.1a):

* keys - names of variables in the TextFSM template
* values - parts of the output that correspond to variables

Check the operation of the function using the output
of the sh ip int br command and devices from devices.yaml.

Task 21.5
~~~~~~~~~~~~

Create function send_and_parse_command_parallel.

The send_and_parse_command_parallel function must run
the send_and_parse_show_command function from task 21.4 in concurrent threads.

Send_and_parse_command_parallel function parameters:

* devices - a list of dicts with connection parameters for devices
* command - command
* templates_path - path to the directory with TextFSM templates
* limit - maximum number of concurrent threads (default 3)

The function should return a dictionary:

* keys - the IP address of the device from which the output was received
* values - a list of dicts (the output returned by the send_and_parse_show_command function)

Dictionary example:

.. code:: python

    {'192.168.100.1': [{'address': '192.168.100.1',
                        'intf': 'Ethernet0/0',
                        'protocol': 'up',
                        'status': 'up'},
                       {'address': '192.168.200.1',
                        'intf': 'Ethernet0/1',
                        'protocol': 'up',
                        'status': 'up'}],
     '192.168.100.2': [{'address': '192.168.100.2',
                        'intf': 'Ethernet0/0',
                        'protocol': 'up',
                        'status': 'up'},
                       {'address': '10.100.23.2',
                        'intf': 'Ethernet0/1',
                        'protocol': 'up',
                        'status': 'up'}]}

Check the operation of the function using the output
of the sh ip int br command and devices from devices.yaml.
