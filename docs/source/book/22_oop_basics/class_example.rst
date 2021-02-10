Class example
~~~~~~~~~~~~~

The class that describes the network:

.. code:: python

    class Network:
        def __init__(self, network):
            self.network = network
            self.hosts = tuple(str(ip) for ip in ipaddress.ip_network(network).hosts())
            self.allocated = []

        def allocate(self, ip):
            if ip in self.hosts:
                if ip not in self.allocated:
                    self.allocated.append(ip)
                else:
                    raise ValueError(f"IP address {ip} is already in the allocated list")
            else:
                raise ValueError(f"IP address {ip} does not belong to {self.network}")

Using the class:

.. code:: python

    In [2]: net1 = Network("10.1.1.0/29")

    In [3]: net1.allocate("10.1.1.1")

    In [4]: net1.allocate("10.1.1.2")

    In [5]: net1.allocated
    Out[5]: ['10.1.1.1', '10.1.1.2']

    In [6]: net1.allocate("10.1.1.100")
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    <ipython-input-6-9a4157e02c78> in <module>
    ----> 1 net1.allocate("10.1.1.100")

    <ipython-input-1-c5255d37a7fd> in allocate(self, ip)
         12             raise ValueError(f"IP address {ip} is already in the allocated list")
         13     else:
    ---> 14         raise ValueError(f"IP address {ip} does not belong to {self.network}")
         15

    ValueError: IP address 10.1.1.100 does not belong to 10.1.1.0/29

    In [7]: net1.hosts
    Out[7]: ('10.1.1.1', '10.1.1.2', '10.1.1.3', '10.1.1.4', '10.1.1.5', '10.1.1.6')

