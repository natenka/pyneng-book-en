Transferring argument to script  (argv)
----------------------------------

Very often script solves some common problem. For example, script processes a configuration file. Of course, in this case you don’t want to edit name of file every time with your hands in script.

It will be much better to pass file name as script argument and then use already specified file.

The sys module allows working with script arguments via argv.

Example of access_template_argv.py:

.. code:: python

    from sys import argv

    interface = argv[1]
    vlan = argv[2]

    access_template = ['switchport mode access',
                       'switchport access vlan {}',
                       'switchport nonegotiate',
                       'spanning-tree portfast',
                       'spanning-tree bpduguard enable']

    print('interface {}'.format(interface))
    print('\n'.join(access_template).format(vlan))

Script test:

::

    $ python access_template_argv.py Gi0/7 4
    interface Gi0/7
    switchport mode access
    switchport access vlan 4
    switchport nonegotiate
    spanning-tree portfast
    spanning-tree bpduguard enable

Arguments that have been passed to script are substituted as values in template.

Several points need to be clarified:

* argv is a list
* all arguments are in list and represented as strings
* argv contains not only arguments that passed to script but also name of script itself

In this case, argv list contains the following elements:

::

    ['access_template_argv.py', 'Gi0/7', '4']

First comes the name of script itself, then arguments in the same order.

