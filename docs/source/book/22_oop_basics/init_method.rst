Method ``__init__``
~~~~~~~~~~~~~~~~~~

For ``info`` method to work correctly the instance should have ``hostname`` and
``model`` variables. If these variables are not available, an error will occur:

.. code:: python

    In [15]: class Switch:
        ...:     def info(self):
        ...:         print('Hostname: {}\nModel: {}'.format(self.hostname, self.model))
        ...:

    In [59]: sw2 = Switch()

    In [60]: sw2.info()
    ---------------------------------------------------------------------------
    AttributeError                            Traceback (most recent call last)
    <ipython-input-60-5a006dd8aae1> in <module>()
    ----> 1 sw2.info()

    <ipython-input-57-30b05739380d> in info(self)
          1 class Switch:
          2     def info(self):
    ----> 3         print('Hostname: {}\nModel: {}'.format(self.hostname, self.model))

    AttributeError: 'Switch' object has no attribute 'hostname'

Almost always, when an object is created it has some initial data. For example,
to create a connection to device with netmiko you have to pass
connection parameters.

In Python these initial object data are specified in ``__init__``.
Method ``__init__`` is executed after Python has created a new instance
and ``__init__`` method is passed arguments with which instance was created:

.. code:: python

    In [32]: class Switch:
        ...:     def __init__(self, hostname, model):
        ...:         self.hostname = hostname
        ...:         self.model = model
        ...:
        ...:     def info(self):
        ...:         print('Hostname: {}\nModel: {}'.format(self.hostname, self.model))
        ...:

Note that each instance created from this class will have variables:
``self.model`` and ``self.hostname``.

Now, when creating an instance of Switch class you have to specify
``hostname`` and ``model``:

.. code:: python

    In [33]: sw1 = Switch('sw1', 'Cisco 3850')

Accordingly, ``info`` method works without error:

.. code:: python

    In [36]: sw1.info()
    Hostname: sw1
    Model: Cisco 3850

.. note::

    ``__init__`` method is sometimes called a class constructor, although
    technically in Python ``__new__`` method is executed first and then
    ``__init__``. In most cases there is no necessety to create
    ``__new__`` method.

An important feature of ``__init__`` method is that it should not return
anything. Python will generate an exception if it tries to do this.

