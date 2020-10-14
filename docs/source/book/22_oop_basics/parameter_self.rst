Parameter self
~~~~~~~~~~~~~

Parameter **self** was specified before in method definition, as well as when using instance variables in method. Parameter **self** is a reference to a particular instance of class. Parameter **self** is not a special name but an arrangement. Instead of **self** you can use a different name but you shouldn't do that.

Example of using a different name instead of **self**:

.. code:: python

    In [15]: class Switch:
        ...:     def info(sw_object):
        ...:         print('Hostname: {}\nModel: {}'.format(sw_object.hostname, sw_object.model))
        ...:

It will work the same way:

.. code:: python

    In [16]: sw1 = Switch()

    In [17]: sw1.hostname = 'sw1'

    In [18]: sw1.model = 'Cisco 3850'

    In [19]: sw1.info()
    Hostname: sw1
    Model: Cisco 3850

.. warning::

    Although technically you can use another name but always use **self**.

In all "usual" methods of class the first parameter will always be **self**. Furthermore, creating an instance variable within a class is also done via **self**.

An example of Switch class with new generate_interfaces method: generate_interfaces method must generate a list with interfaces based on specified type and quantity and create variable in an instance of class. First, an option of creating a usual variable within method:

.. code:: python

    In [5]: class Switch:
       ...:     def generate_interfaces(self, intf_type, number_of_intf):
       ...:         interfaces = ['{}{}'.format(intf_type, number) for number in range(1, number_of_intf+1)]
       ...:

In this case, class instances will not have *interfaces* variable:

.. code:: python

    In [6]: sw1 = Switch()

    In [7]: sw1.generate_interfaces('Fa', 10)

    In [8]: sw1.interfaces
    ---------------------------------------------------------------------------
    AttributeError                            Traceback (most recent call last)
    <ipython-input-8-e6b457e4e23e> in <module>()
    ----> 1 sw1.interfaces

    AttributeError: 'Switch' object has no attribute 'interfaces'

This variable does not exist because it exists only within method and visibility area of method is the same as function. Even other methods of the same class do not see variables in other methods.

For list with interfaces to be available as a variable in instances, you have to assign value in self.interfaces:

.. code:: python

    In [9]: class Switch:
       ...:     def info(self):
       ...:         print('Hostname: {}\nModel: {}'.format(self.hostname, self.model))
       ...:
       ...:     def generate_interfaces(self, intf_type, number_of_intf):
       ...:         interfaces = ['{}{}'.format(intf_type, number) for number in range(1, number_of_intf+1)]
       ...:         self.interfaces = interfaces
       ...:

Now, after generate_interfaces method is called *interfaces* variable is created in instance:

.. code:: python

    In [10]: sw1 = Switch()

    In [11]: sw1.generate_interfaces('Fa', 10)

    In [12]: sw1.interfaces
    Out[12]: ['Fa1', 'Fa2', 'Fa3', 'Fa4', 'Fa5', 'Fa6', 'Fa7', 'Fa8', 'Fa9', 'Fa10']

