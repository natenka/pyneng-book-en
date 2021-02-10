Class variables
~~~~~~~~~~~~~~~~~

Besides instance variables, there are also class variables. They are
are created by specifying variables inside the class itself, not a method:

.. code:: python

    class Network:
        all_allocated_ip = []

        def __init__(self, network):
            self.network = network
            self.hosts = tuple(
                str(ip) for ip in ipaddress.ip_network(network).hosts()
            )
            self.allocated = []


        def allocate(self, ip):
            if ip in self.hosts:
                if ip not in self.allocated:
                    self.allocated.append(ip)
                    type(self).all_allocated_ip.append(ip)
                else:
                    raise ValueError(f"IP address {ip} is already in the allocated list")
            else:
                raise ValueError(f"IP address {ip} does not belong to {self.network}")

Class variables can be accessed in different ways:

* ``self.all_allocated_ip``
* ``Network.all_allocated_ip``
* ``type(self).all_allocated_ip``


The ``self.all_allocated_ip`` option allows you to access the value of class variable
or add an element if the class variable is a mutable data type.
The disadvantage of this option is that if in the method you write
``self.all_allocated_ip = ...``, instead of changing the class variable,
an instance variable will be created.

Вариант ``Network.all_allocated_ip`` будет работать корректно, но небольшой минус
этого варианта в том, что имя класса прописано вручную.
Вместо него можно использовать третий вариант ``type(self).all_allocated_ip``,
так как ``type(self)`` возвращает класс.


The option ``Network.all_allocated_ip`` will work correctly, but a small drawback
this option is that the class name is written manually.
You can use the third option ``type(self).all_allocated_ip`` instead,
since ``type(self)`` returns a class.

Now the class has a variable all_allocated_ip which is written
all IP addresses that are allocated on the networks:

.. code:: python

    In [3]: net1 = Network("10.1.1.0/29")

    In [4]: net1.allocate("10.1.1.1")
       ...: net1.allocate("10.1.1.2")
       ...: net1.allocate("10.1.1.3")
       ...:

    In [5]: net1.allocated
    Out[5]: ['10.1.1.1', '10.1.1.2', '10.1.1.3']

    In [6]: net2 = Network("10.2.2.0/29")

    In [7]: net2.allocate("10.2.2.1")
       ...: net2.allocate("10.2.2.2")
       ...:

    In [9]: net2.allocated
    Out[9]: ['10.2.2.1', '10.2.2.2']

    In [10]: Network.all_allocated_ip
    Out[10]: ['10.1.1.1', '10.1.1.2', '10.1.1.3', '10.2.2.1', '10.2.2.2']

The variable is accessible not only through the class, but also through the instances:

.. code:: python

    In [40]: Network.all_allocated_ip
    Out[40]: ['10.1.1.1', '10.1.1.2', '10.1.1.3', '10.2.2.1', '10.2.2.2']

    In [41]: net1.all_allocated_ip
    Out[41]: ['10.1.1.1', '10.1.1.2', '10.1.1.3', '10.2.2.1', '10.2.2.2']

    In [42]: net2.all_allocated_ip
    Out[42]: ['10.1.1.1', '10.1.1.2', '10.1.1.3', '10.2.2.1', '10.2.2.2']


