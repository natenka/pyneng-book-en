Create your own modules
----------------------

Module is a file with . py extension and Python code.

Example of creating your own modules and importing a function from one module to another.

File check_ip_function.py:

.. code:: python

    import ipaddress


    def check_ip(ip):
        try:
            ipaddress.ip_address(ip)
            return True
        except ValueError as err:
            return False


    ip1 = '10.1.1.1'
    ip2 = '10.1.1'

    print('Checking IP...')
    print(ip1, check_ip(ip1))
    print(ip2, check_ip(ip2))

File check_ip_function.py has created check_ip function which checks that argument is an IP address. This is done by using **ipaddress** module which will be discussed in the next section.

Function ipaddress.ip_address() itself checks the correctness of IP address and generates ValueError exception if address is not validated.

Function check_ip() returns True if address is validated and False if not.

If you run check_ip_function.py script, the output is:

::

    $ python check_ip_function.py
    Checking IP...
    10.1.1.1 True
    10.1.1 False


The second script imports check_ip() function and uses it to select from address list only those that passed the check (get_correct_ip.py file):

.. code:: python

    from check_ip_function import check_ip


    def return_correct_ip(ip_addresses):
        correct = []
        for ip in ip_addresses:
            if check_ip(ip):
                correct.append(ip)
        return correct

    print('Cheking list of IP addresses')
    ip_list = ['10.1.1.1', '8.8.8.8', '2.2.2']
    correct = return_correct_ip(ip_list)
    print(correct)


First line imports check_ip function from check_ip_function.py module.

Result of script execution:

::

    $ python get_correct_ip.py
    Cheking IP...
    10.1.1.1 True
    10.1.1 False
    Cheking list of IP addresses
    ['10.1.1.1', '8.8.8.8']

Note that not only information from get_correct_ip.py script is displayed, but also information from check_ip_function.py. This is because any type of import executes the entire script. That is, even when import looks like ``from check_ip_function import check_ip``, entire check_ip_function.py script is executed, not just check_ip function. As a result, all messages of imported script will be displayed.

Messages from imported script are not scary, they are just confusing. Worse when script performed some kind of connection to equipment and when importing a function from it, we will have to wait for connection to take place.

Python can specify that some strings should not be executed when importing. This is discussed in the following subsection.

.. note::
    Function ``return_correct_ip`` can be replaced by a ``filter`` or a list comprehension.
    Above is used the longer but most likely more understandable option:

    .. code:: python

        In [19]: list(filter(check_ip, ip_list))
        Out[19]: ['10.1.1.1', '8.8.8.8']

        In [20]: [ip for ip in ip_list if check_ip(ip)]
        Out[20]: ['10.1.1.1', '8.8.8.8']

        In [21]: def return_correct_ip(ip_addresses):
            ...:     return [ip for ip in ip_addresses if check_ip(ip)]
            ...:

        In [22]: return_correct_ip(ip_list)
        Out[22]: ['10.1.1.1', '8.8.8.8']

