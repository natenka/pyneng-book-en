.. raw:: latex

   \newpage

.. _oop_special_methods_index:

23. Special methods
======================

Special methods in Python - methods that are responsible for  "standard"
possibilities of objects and are called automatically when these
possibilities are used. For example, expression ``a + b``  where a and b
are numbers that is converted to such a call  ``a.__add__(b)``. That is,
special method ``__add__`` is responsible for the addition operation.
All special methods start and end with double underscore, therefore in
English they are often called dunder methods, shortened from "double underscore".


.. note::

    Special methods are often called magic methods.

Special methods are responsible for such features as working in context
managers, creating iterators and iterable objects, addition operations,
multiplication and others. By adding special methods to objects that
are created by user, we make them look like built in Python objects.

.. toctree::
   :maxdepth: 1

   underscore_names
   str_repr
   add_method
   protocols
   ../../exercises/23_exercises.rst
