Naming convention
--------------------

Python has certain objects naming convention

In general, it is better to adhere to this convention. However, if a particular library or module uses different convention, it is worth following the style used in them.

    Not all rules are described in this section. More information can be found in PEP8 in 
    `English <https://www.python.org/dev/peps/pep-0008/>`__ or
    `Russian <http://pep8.ru/doc/pep8/>`__.

Variable names
~~~~~~~~~~~~~~~~

Variable names should not overlap with operators and names of modules or other reserved values.

Variable names are usually written entirely in large or small letters. It is better to stick to one of option within a script/module/package.

If variables are constants for module, it is better to use names written in capital letters:

.. code:: python

    DB_NAME = 'dhcp_snooping.db'
    TESTING = True

For ordinary variables it is better to use lower case names:

.. code:: python

    db_name = 'dhcp_snooping.db'
    testing = True

Module and package names
~~~~~~~~~~~~~~~~~~~~~~~

Names of modules and packages are given in small letters.

Modules can use underscores to make names more understandable. For packages it is better to select short names.

Function names
~~~~~~~~~~~~~

Function names are given in small letters with underscores between words.

.. code:: python

    def ignore_command(command, ignore):

        ignore_command = False

        for word in ignore:
            if word in command:
                return True
        return ignore_command

Class names
~~~~~~~~~~~~~

Class names are given with capital letters, no spaces.

.. code:: python

    class CiscoSwitch:
        
        def __init__(self, name, vendor = 'cisco', model = '3750'):
            self.name = name
            self.vendor = vendor
            self.model = model

