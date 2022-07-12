Protocols
---------

Special methods are responsible not only for support of operations like
addition and comparison, but also for protocol support. Protocol - set
of methods that must be implemented in object to make object support a
certain behavior. For example, Python has protocols like iteration,
context manager, containers and others. After creating certain methods
in the object, it will behave as built-in and use an interface understood
by all who write on Python.

.. note::

    `A table with abstract classes describing which methods an object should have to make it support a certain protocol <https://docs.python.org/3/library/collections.abc.html#collections-abstract-base-classes>`__


.. toctree::
   :maxdepth: 1
   :hidden:

   iterable_iterator
   sequence_protocol
   context_manager
