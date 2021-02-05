Method creation
~~~~~~~~~~~~~~~

Before we start dealing with class methods, let's see an example of a function
that waits as an argument an instance variable of Switch class and displays
information about it using instance variables ``hostname`` and ``model``:

.. code:: python

    In [1]: def info(sw_obj):
       ...:     print('Hostname: {}\nModel: {}'.format(sw_obj.hostname, sw_obj.model))
       ...:

    In [2]: sw1 = Switch()

    In [3]: sw1.hostname = 'sw1'

    In [4]: sw1.model = 'Cisco 3850'

    In [5]: info(sw1)
    Hostname: sw1
    Model: Cisco 3850

In ``info`` function, ``sw_obj`` awaits an instance of ``Switch`` class.
Most likely, there is nothing new about this example, because in the same way
earlier we wrote functions that wait for a string as an argument and then call
some methods in this string.

This example will help you to understand ``info`` method that we will add to Switch class.

To add a method you have to create a function within class:

.. code:: python

    In [15]: class Switch:
        ...:     def info(self):
        ...:         print('Hostname: {}\nModel: {}'.format(self.hostname, self.model))
        ...:

If you look closely, ``info`` method looks exactly like ``info`` function, only
instead of ``sw_obj`` name the ``self`` is used. Why there is a strange ``self``
name here will be explained later and in the meantime we will see how to call
``info`` method:

.. code:: python

    In [16]: sw1 = Switch()

    In [17]: sw1.hostname = 'sw1'

    In [18]: sw1.model = 'Cisco 3850'

    In [19]: sw1.info()
    Hostname: sw1
    Model: Cisco 3850

In example above, first an instance of Switch class is created,
then ``hostname`` and ``model`` variables are added to instance and
then ``info`` method is called. Method ``info`` outputs information
about switch using values that are stored in instance variables.

Method call is different from the function call: we do not pass a link to an
instance of ``Switch`` class. We don't need that because we call method from
instance itself. Another unclear thing - why we wrote ``self`` then?

The point is that Python transforms such a call:

.. code:: python

    In [39]: sw1.info()
    Hostname: sw1
    Model: Cisco 3850

To this one:

.. code:: python

    In [38]: Switch.info(sw1)
    Hostname: sw1
    Model: Cisco 3850

In the second case, ``self`` parameter already makes more sense, it actually
accepts the reference to instance and displays information on this basis.

From objects usage point of view, it is more convenient to call methods
using the first syntax version, so it is almost always used.

.. note::

    When a class instance method is called the instance reference is passed
    by the first argument. In this case, instance is passed implicitly but
    parameter must be stated explicitly.

This conversion is not a feature of user classes and works for embedded data
types in the same way. For example, standard way to call ``append`` method
in the list is:

.. code:: python

    In [4]: a = [1,2,3]

    In [5]: a.append(5)

    In [6]: a
    Out[6]: [1, 2, 3, 5]

The same can be done using the second option, calling through a class:

.. code:: python

    In [7]: a = [1,2,3]

    In [8]: list.append(a, 5)

    In [9]: a
    Out[9]: [1, 2, 3, 5]

