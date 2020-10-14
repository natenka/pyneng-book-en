Iteration protocol
~~~~~~~~~~~~~~~~~


**iterable object (iterable)** - object that can return elements one at a time. 
For Python, it is any object that has ``__iter__`` or ``__getitem__`` method.
If an object has __iter__ method, the iterated object becomes an iterator by calling ``iter(name)`` where *name* - name of iterable object. If __iter__ method is not present, Python iterates elements using __getitem__.


.. code:: python

    class Items:
        def __init__(self, items):
            self.items = items

        def __getitem__(self, index):
            print('Вызываю __getitem__')
            return self.items[index]


    In [2]: iterable_1 = Items([1, 2, 3, 4])

    In [3]: iterable_1[0]
    Calling __getitem__
    Out[3]: 1

    In [4]: for i in iterable_1:
       ...:     print('>>>>', i)
       ...:
    Calling __getitem__
    >>>> 1
    Calling __getitem__
    >>>> 2
    Calling __getitem__
    >>>> 3
    Calling __getitem__
    >>>> 4
    Calling __getitem__

    In [5]: list(map(str, iterable_1))
    Calling __getitem__
    Calling __getitem__
    Calling __getitem__
    Calling __getitem__
    Calling __getitem__
    Out[5]: ['1', '2', '3', '4']

If object has __iter__ method (which must return iterator), it is used for values iteration:

.. code:: python

    class Items:
        def __init__(self, items):
            self.items = items

        def __getitem__(self, index):
            print('Вызываю __getitem__')
            return self.items[index]

        def __iter__(self):
            print('Вызываю __iter__')
            return iter(self.items)


    In [12]: iterable_1 = Items([1, 2, 3, 4])

    In [13]: for i in iterable_1:
         ...:     print('>>>>', i)
         ...:
    Calling __iter__
    >>>> 1
    >>>> 2
    >>>> 3
    >>>> 4

    In [14]: list(map(str, iterable_1))
    Calling __iter__
    Out[14]: ['1', '2', '3', '4']

In Python, iter() function is responsible for getting an iterator :

.. code:: python

    In [1]: lista = [1, 2, 3]

    In [2]: iter(lista)
    Out[2]: <list_iterator at 0xb4ede28c>

``iter`` function will work on any object that has __iter__ or __getitem__ method.
Method __iter__ returns an iterator. If this method is not available, iter() function checks availability of __getitem__ method that can get elements by index. If __getitem__ method exists, elements will be iterated through index (starting with 0).


**iterator** - object that returns its elements one at a time.
From Python point of view, it is any object that has __next__method. This method returns the next item if any or returns Stopiteration exception when items are ended. In addition, iterator remembers which object it stopped at in the last iteration. Each iterator also has __iter__ method - that is, every iterator is an iterable object. This method returns iterator itself.

An example of creating iterator from list:

.. code:: python

    In [3]: lista = [1, 2, 3]

    In [4]: i = iter(lista)

Now you can use next() function that calls __next__method to take the next element:

.. code:: python

    In [5]: next(i)
    Out[5]: 1

    In [6]: next(i)
    Out[6]: 2

    In [7]: next(i)
    Out[7]: 3

    In [8]: next(i)
    ------------------------------------------------------------
    StopIteration              Traceback (most recent call last)
    <ipython-input-8-bed2471d02c1> in <module>()
    ----> 1 next(i)

    StopIteration:

After elements are ended, Stopiteration exception is returned. In order for iterator to return elements again, it has to be re-created. Similar actions are performed when **for** loop iterates items in the list:

.. code:: python

    In [9]: for item in lista:
       ...:     print(item)
       ...:
    1
    2
    3

When we iterate list items, iter() function is first applied to the list to create an iterator and then __next__ method is called until Stopiteration exception occurs.

An example of my_for() function that works with any iterable object and imitates built-in function **for**:

.. code:: python

    def my_for(iterable):
        if getattr(iterable, "__iter__", None):
            print('Есть __iter__')
            iterator = iter(iterable)
            while True:
                try:
                    print(next(iterator))
                except StopIteration:
                    break
        elif getattr(iterable, "__getitem__", None):
            print('Нет __iter__, но есть __getitem__')
            index = 0
            while True:
                try:
                    print(iterable[index])
                    index += 1
                except IndexError:
                    break

Check function on object that has __iter__:

.. code:: python

    In [18]: my_for([1,2,3,4])
    Есть __iter__
    1
    2
    3
    4

Check function on object that does not have __iter__ but has __getitem__:

.. code:: python

    class Items:
        def __init__(self, items):
            self.items = items

        def __getitem__(self, index):
            print('Вызываю __getitem__')
            return self.items[index]


    In [20]: iterable_1 = Items([1,2,3,4,5])

    In [21]: my_for(iterable_1)
    Нет __iter__, но есть __getitem__
    Calling __getitem__
    1
    Calling __getitem__
    2
    Calling __getitem__
    3
    Calling __getitem__
    4
    Calling __getitem__
    5
    Calling __getitem__


Iterator creation
^^^^^^^^^^^^^^^^^^

Example of Network class:

.. code:: python

    In [10]: import ipaddress
        ...:
        ...: class Network:
        ...:     def __init__(self, network):
        ...:         self.network = network
        ...:         subnet = ipaddress.ip_network(self.network)
        ...:         self.addresses = [str(ip) for ip in subnet.hosts()]

Example of Network class instance creation:

.. code:: python

    In [14]: net1 = Network('10.1.1.192/30')

    In [15]: net1
    Out[15]: <__main__.Network at 0xb3124a6c>

    In [16]: net1.addresses
    Out[16]: ['10.1.1.193', '10.1.1.194']

    In [17]: net1.network
    Out[17]: '10.1.1.192/30'

Create an iterator from Network class:

.. code:: python

    In [12]: class Network:
        ...:     def __init__(self, network):
        ...:         self.network = network
        ...:         subnet = ipaddress.ip_network(self.network)
        ...:         self.addresses = [str(ip) for ip in subnet.hosts()]
        ...:         self._index = 0
        ...:
        ...:     def __iter__(self):
        ...:         print('Вызываю __iter__')
        ...:         return self
        ...:
        ...:     def __next__(self):
        ...:         print('Вызываю __next__')
        ...:         if self._index < len(self.addresses):
        ...:             current_address = self.addresses[self._index]
        ...:             self._index += 1
        ...:             return current_address
        ...:         else:
        ...:             raise StopIteration
        ...:

Method __iter__ in iterator must return object itself, therefore  ``return self`` is specified in method and __next__ method returns elements one at a time and generates Stoeratipiton exception when elements have run out.


.. code:: python

    In [14]: net1 = Network('10.1.1.192/30')

    In [15]: for ip in net1:
        ...:     print(ip)
        ...:
    Calling __iter__
    Calling __next__
    10.1.1.193
    Calling __next__
    10.1.1.194
    Calling __next__

Most of the time, iterator is a disposable object and once we’ve iterated elements, we can’t do it again:

.. code:: python

    In [16]: for ip in net1:
        ...:     print(ip)
        ...:
    Calling __iter__
    Calling __next__


Creation of iterable object
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Very often it is sufficient for class to be an iterable object and not necessarily an iterator. If an object is iterable, it can be used in *for* loop, *map* functions, *filter*, *sorted*, *enumerate* and others. It is also generally easier to make an iterable object than an iterator.

In order for Network class to create iterable objects, class must have __iter__ (__next__ is not needed) and method must return iterator. Since in this case, Network iterates addresses that are in self.addresses list, the easiest option to return iterator is to return  ``iter(self.addresses)``:

.. code:: python

    In [17]: class Network:
        ...:     def __init__(self, network):
        ...:         self.network = network
        ...:         subnet = ipaddress.ip_network(self.network)
        ...:         self.addresses = [str(ip) for ip in subnet.hosts()]
        ...:
        ...:     def __iter__(self):
        ...:         return iter(self.addresses)
        ...:

Now all Network class instances will be iterable objects:

.. code:: python

    In [18]: net1 = Network('10.1.1.192/30')

    In [19]: for ip in net1:
        ...:     print(ip)
        ...:
    10.1.1.193
    10.1.1.194

