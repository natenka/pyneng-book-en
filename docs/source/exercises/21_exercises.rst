.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 21.1
~~~~~~~~~~~~

Create parse_command_output() function. Function parameters:

* template - name of file containing Textfsm template  (templates/sh_ip_int_br.template)
* command_output - output of corresponding show command (string)

Function should return list:

* first element - list with column names
* other elements - lists containing results of output processing 

Check function with output/sh_ip_int_br.txt and templates/sh_ip_int_br.template template.

.. code:: python

    from netmiko import ConnectHandler


    # function call should be like
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

Create parse_output_to_dict() function.

Function parameters:

* template - name of file containing Textfsm template (templates/sh_ip_int_br.template)
* command_output - output of corresponding show command (string)

Function should return list of dictionaries:

* keys - variable names in Textfsm template
* values  - parts of output that correspond to variables

Check function with output/sh_ip_int_br.txt and templates/sh_ip_int_br.template.

Task 21.2
~~~~~~~~~~~~

Create Textfsm template for processing output of *sh ip dhcp snooping binding* and write it to templates/sh_ip_dhcp_snooping.template

Output of command is in output/sh_ip_dhcp_snooping.txt.

Template should process and return values of such columns:

* mac - like 00:04:A3:3E:5B:69
* ip - like 10.1.10.6
* vlan - 10
* intf - FastEthernet0/10

Check template with parse_command_output() function from task 21.1.

Task 21.3
~~~~~~~~~~~~

Create parse_command_dynamic() function.

Function parameters:

* command_output - command output (string)
* attributes_dict - dictionary with attributes containing such key-value pairs:

  * 'Command': command
  * 'Vendor': vendor

* index_file - name of file where mapping between commands and templates is stored. Default value - "index"
* templ_path - directory where templates are stored. Default value is - "templates"

Function should return list of dictionaries with output results (as in 21.1a):

* keys - variable names in Textfsm template
* values  - parts of output that correspond to variables

Check function with *sh ip int br* output.

Task 21.4
~~~~~~~~~~~~

Create send_and_parse_show_command() function.

Function parameters:

* device_dict - dictionary with connection parameters to one device
* command - command to execute
* templates_path - path to Textfsm template directory
* index - name of index file, default "index"

Function should connect to a single device, send show command with netmiko and then parse command output with Textfsm.

Function should return a list of dictionaries with output results (as in 21.1a):

* keys - variable names in Textfsm template
* values  - parts of output that correspond to variables

Check function with *sh ip int br* output and devices from devices.yaml.

Task 21.5
~~~~~~~~~~~~

Create send_and_parse_command_parallel() function.

Function send_and_parse_command_parallel() should call send_and_parse_show_command() functon in parallel threads from task 21.4.

In this task, you have to decide:

* what parameters will be used in function
* what function will return


There’s no test for this task.
