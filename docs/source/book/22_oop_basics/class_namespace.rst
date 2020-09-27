Visibility area
~~~~~~~~~~~~~~~~~

Each method in class has its own local visibility area. This means that one class method does not see variables of another class method. For variables to be available, you have to assign their instance through  ``self.name``.
Basically, method is a function tied to an object. Therefore, all nuances that concern function apply to methods.

Variable instances are available in another method because instance itself is passed as a first argument to each method. In the example below in  ``__init__`` method, *hostname* and *model* variables are assigned to an instance and then used in info() due to the instance being passed as a first argument:

.. code:: python

    In [32]: class Switch:
        ...:     def __init__(self, hostname, model):
        ...:         self.hostname = hostname
        ...:         self.model = model
        ...:
        ...:     def info(self):
        ...:         print('Hostname: {}\nModel: {}'.format(self.hostname, self.model))
        ...:

