Pytest basics
-------------

First, you need to install pytest and pyyaml:

::

    pip install pytest
    pip install pyyaml

Although you don’t have to write tests code but to understand it you should look at an example of a test. For example, there is the following code with check_ip() function:

.. code:: python

    import ipaddress


    def check_ip(ip):
        try:
            ipaddress.ip_address(ip)
            return True
        except ValueError as err:
            return False


    if __name__ == "__main__":
        result = check_ip('10.1.1.1')
        print('Function result:', result)

Function check_ip() checks whether the argument given to it is an IP address. An example of calling a function with different arguments:

.. code:: python

    In [1]: import ipaddress
       ...:
       ...:
       ...: def check_ip(ip):
       ...:     try:
       ...:         ipaddress.ip_address(ip)
       ...:         return True
       ...:     except ValueError as err:
       ...:         return False
       ...:

    In [2]: check_ip('10.1.1.1')
    Out[2]: True

    In [3]: check_ip('10.1.')
    Out[3]: False

    In [4]: check_ip('a.a.a.a')
    Out[4]: False

    In [5]: check_ip('500.1.1.1')
    Out[5]: False

Now it is necessary to write a test for check_ip() function. Test must check that function returns True when correct address is passed and False when wrong argument is passed.

To simplify task, test can be written in the same file. In pytest, test can be a normal function with a name that starts with *test_*. Inside function you have to write conditions that are checked. In pytest this is done with *assert*.

assert
~~~~~~

*assert* does nothing if expression is True and generates an exception if expression is False:

.. code:: python

    In [6]: assert 5 > 1

    In [7]: a = 4

    In [8]: assert a in [1,2,3,4]

    In [9]: assert a not in [1,2,3,4]
    ---------------------------------------------------------------------------
    AssertionError                            Traceback (most recent call last)
    <ipython-input-9-1956288e2d8e> in <module>
    ----> 1 assert a not in [1,2,3,4]

    AssertionError:

    In [10]: assert 5 < 1
    ---------------------------------------------------------------------------
    AssertionError                            Traceback (most recent call last)
    <ipython-input-10-b224d03aab2f> in <module>
    ----> 1 assert 5 < 1

    AssertionError:

After *assert* and expression you can write a message. If there is a message, it is displayed in exception:

.. code:: python

    In [11]: assert a not in [1,2,3,4], "a not in a list"
    ---------------------------------------------------------------------------
    AssertionError                            Traceback (most recent call last)
    <ipython-input-11-7a8f87272a54> in <module>
    ----> 1 assert a not in [1,2,3,4], "a not in a list"

    AssertionError: a not in a list

Test example
~~~~~~~~~~~~

pytest uses *assert* to specify which conditions must be met in order for test to be considered passed.

In pytest, you can write test as a normal function but function name must start with *test_*. Below is test_check_ip test which verify check_ip() function by passing two values to it: correct address and wrong one, and after each check the message is written:

.. code:: python

    import ipaddress


    def check_ip(ip):
        try:
            ipaddress.ip_address(ip)
            return True
        except ValueError as err:
            return False


    def test_check_ip():
        assert check_ip('10.1.1.1') == True, 'If IP is correct, the fucntion returns True'
        assert check_ip('500.1.1.1') == False, 'If IP is wrong, the fucntion returns False'


    if __name__ == "__main__":
        result = check_ip('10.1.1.1')
        print('Function result:', result)

Code is written in check_ip_functions.py. Now you have to figure out how to call tests. The easiest option is to write *pytest* word. In this case, pytest will automatically detect tests in the current directory. However, pytest has certain rules, not only by name of function but also by name of test files - file names should also start with *test_*. If rules are respected, pytest will automatically find tests, if not - you have to specify a test file.

In the case of example above, you have to call a command:

::

    $ pytest check_ip_functions.py
    ========================= test session starts ==========================
    platform linux -- Python 3.7.3, pytest-4.6.2, py-1.5.2, pluggy-0.12.0
    rootdir: /home/vagrant/repos/general/pyneng.github.io/code_examples/pytest
    collected 1 item

    check_ip_functions.py .                                          [100%]

    ======================= 1 passed in 0.02 seconds =======================

By default if tests pass, each test (test_check_ip function) is marked with a dot. Since in this case there is only one test - test_check_ip()function, there is a dot after name check_ip_functions.py and it is also written below that 1 test has passed.

Now, suppose the function does not work correctly and always returns False (write return False at the beginning of function). In this case, test execution will look like:

::

    $ pytest check_ip_functions.py
    ========================= test session starts ==========================
    platform linux -- Python 3.6.3, pytest-4.6.2, py-1.5.2, pluggy-0.12.0
    rootdir: /home/vagrant/repos/general/pyneng.github.io/code_examples/pytest
    collected 1 item

    check_ip_functions.py F                                          [100%]

    =============================== FAILURES ===============================
    ____________________________ test_check_ip _____________________________

        def test_check_ip():
    >       assert check_ip('10.1.1.1') == True, 'If IP is correct, the fucntion returns True'
    E       AssertionError: If IP is correct, the fucntion returns True
    E       assert False == True
    E        +  where False = check_ip('10.1.1.1')

    check_ip_functions.py:14: AssertionError
    ======================= 1 failed in 0.06 seconds =======================

If test fails, pytest displays more information and shows where things went wrong. In this case, after execution of ``assert check_ip('10.1.1.1') == True`` string, the expression did not return True result, so an exception was generated.

Below, pytest shows what it has compared:
``assert False == True`` and specifies that False is  ``check_ip('10.1.1.1')``. Looking at the output, one suspects that something is wrong with check_ip() function because it returns False to correct address.

Most tests are written in separate files. For this example, test is only one but it is still in a separate file.

File test_check_ip_function.py:

.. code:: python

    from check_ip_functions import check_ip


    def test_check_ip():
        assert check_ip('10.1.1.1') == True, 'If IP is correct, the fucntion returns True'
        assert check_ip('500.1.1.1') == False, 'If IP is wrong, the fucntion returns False'

File check_ip_functions.py:

.. code:: python

    import ipaddress


    def check_ip(ip):
        #return False
        try:
            ipaddress.ip_address(ip)
            return True
        except ValueError as err:
            return False


    if __name__ == "__main__":
        result = check_ip('10.1.1.1')
        print('Function result:', result)

In that case, test can be run without specifying a file:

::

    $ pytest
    ================= test session starts ========================
    platform linux -- Python 3.6.3, pytest-4.6.2, py-1.5.2, pluggy-0.12.0
    rootdir: /home/vagrant/repos/general/pyneng.github.io/code_examples/pytest
    collected 1 item

    test_check_ip_function.py .                              [100%]

    ================= 1 passed in 0.02 seconds ====================
