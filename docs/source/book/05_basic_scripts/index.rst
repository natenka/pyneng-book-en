.. raw:: latex

   \newpage

.. _basic_scripts_index:

5. Basic scripts
================

Generally speaking, script is a regular file. This file stores the sequence of commands that you want to execute.

Let's start with basic script and print several strings on standard output.
To do this, you need to create an access_template.py file with this content:

.. code:: python

    access_template = ['switchport mode access',
                       'switchport access vlan {}',
                       'switchport nonegotiate',
                       'spanning-tree portfast',
                       'spanning-tree bpduguard enable']

    print('\n'.join(access_template).format(5))

First, items in list are combined into a string that is separated by ``\n``
and VLAN number is inserted into string using string formatting.
After this you should save file and go to command line.
This is how the script execution looks like:

.. code:: python

    $ python access_template.py
    switchport mode access
    switchport access vlan 5
    switchport nonegotiate
    spanning-tree portfast
    spanning-tree bpduguard enable


All scripts that will be created in this course have an extension ``.py``. You can say that it is a «good manners» - to create Python scripts with .py extension.

.. toctree::
   :maxdepth: 1
   :hidden:

   executable
   args
   user_input
   ../../exercises/05_exercises
