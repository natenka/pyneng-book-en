``if __name__ == "__main__"``
-----------------------------

Often the script can be executed independently and can be imported as a module by another script. Since importing a script runs this script, it is often necessary to specify that some strings should not be executed when importing.

In the previous example there were two scripts: check_ip_function.py and get_correct_ip.py. And when starting get_correct_ip.py, print() from check_ip_function.py was displayed.


Python has a special technique that specifies that a code must not be executed at import: all lines that are in the ``if __name__ == '__main__'`` block are not executed at import.

The variable ``__name__`` is a special variable that will be equal to ``"__main__"`` only if the file is run as the main program and is set equal to the module name when importing the module. That is, the ``if __name__ == '__main__'`` condition checks whether the file was run directly.

As a rule, the ``if __name__ == '__main__'`` block includes all function calls and information output on the standard output stream. That is, in the check_ip_function.py script this block conytains everything except import and the return_correct_ip function:

.. code:: python

    import ipaddress


    def check_ip(ip):
        try:
            ipaddress.ip_address(ip)
            return True
        except ValueError as err:
            return False


    if __name__ == '__main__':
        ip1 = '10.1.1.1'
        ip2 = '10.1.1'

        print('Cheking IP...')
        print(ip1, check_ip(ip1))
        print(ip2, check_ip(ip2))


Result of script execution:

::

    $ python check_ip_function.py
    Cheking IP...
    10.1.1.1 True
    10.1.1 False

When you start the check_ip_function.py script directly, all lines are executed, because the variable ``__name__`` in this case is equal to ``'__main__'``.

The get_correct_ip.py script remains unchanged

.. code:: python

    from check_ip_function import check_ip


    def return_correct_ip(ip_addresses):
        correct = []
        for ip in ip_addresses:
            if check_ip(ip):
                correct.append(ip)
        return correct


    print('Checking list of IP addresses')
    ip_list = ['10.1.1.1', '8.8.8.8', '2.2.2']
    correct = return_correct_ip(ip_list)
    print(correct)


Execution of the get_correct_ip.py script:

::

    $ python get_correct_ip.py
    Checking list of IP addresses
    ['10.1.1.1', '8.8.8.8']

Now the output contains only information from the script getcorrect__ip.py.


In general, it is better to write all the code that calls functions and outputs something to the standard output stream inside the block ``if __name__ == '__main__'``.

.. warning::
    Starting with Section 9, there are program tests for tasks that can be used to check whether the tasks are properly executed. To work correctly with tests you have to always write a function call in the job file within the block ``if __name__ == '__main__'``. The absence of this block will cause errors, not in all tasks, but it will still avoid problems.

