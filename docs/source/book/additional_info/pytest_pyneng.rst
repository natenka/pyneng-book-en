Specifics of using pytest to check tasks
----------------------------------------

Pytest in course is primarily used for self-tests of tasks. However, this
test is not optional - task is considered done when it complies with all
specified points and passes tests. For my part, I also check tasks with
automatic tests and then look at the code, write comments if necessary
and show a solution option.

At first, tests require effort but through a couple of sections they
will help solve tasks.

.. warning::

    Tests that are written for course are not a benchmark or best practice of
    test writing. Tests are written with maximum emphasis on clarity and many
    things are done differently.

When solving tasks especially when there are doubts about the final format of
data to be obtained, it is better to look into test. For example,
if task_9_1.py the corresponding test will be in test/test_task_9_1.py.

Test example tests/test_task_9_1.py:

.. code:: python

    import pytest
    import task_9_1
    import sys
    sys.path.append('..')

    from common_functions import check_function_exists, check_function_params


    # Checks is function generate_access_config is created in task task_9_1
    def test_function_created():
        check_function_exists(task_9_1, 'generate_access_config')

    # Cheks fucntion parameters
    def test_function_params():
        check_function_params(function=task_9_1.generate_access_config,
                              param_count=2, param_names=['intf_vlan_mapping', 'access_template'])


    def test_function_return_value():
        access_vlans_mapping = {
            'FastEthernet0/12': 10,
            'FastEthernet0/14': 11,
            'FastEthernet0/16': 17
        }
        template_access_mode = [
            'switchport mode access', 'switchport access vlan',
            'switchport nonegotiate', 'spanning-tree portfast',
            'spanning-tree bpduguard enable'
        ]
        correct_return_value = ['interface FastEthernet0/12',
                                'switchport mode access',
                                'switchport access vlan 10',
                                'switchport nonegotiate',
                                'spanning-tree portfast',
                                'spanning-tree bpduguard enable',
                                'interface FastEthernet0/14',
                                'switchport mode access',
                                'switchport access vlan 11',
                                'switchport nonegotiate',
                                'spanning-tree portfast',
                                'spanning-tree bpduguard enable',
                                'interface FastEthernet0/16',
                                'switchport mode access',
                                'switchport access vlan 17',
                                'switchport nonegotiate',
                                'spanning-tree portfast',
                                'spanning-tree bpduguard enable']

        return_value = task_9_1.generate_access_config(access_vlans_mapping, template_access_mode)
        assert return_value != None, "Functon returns nothing"
        assert type(return_value) == list, "Function has to return a list"
        assert return_value == correct_return_value, "Function return wrong value"

Note correct_return_value variable - this variable contains the resulting list
that should return generate_access_config function. Therefore for example, if
question has arisen of whether to add spaces before commands or a new line at
the end, you can look at what the result requires. Also check your output
against the output in variable_return_value.

How to run tests for tasks verification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The most important thing is where to run tests: all tests must be run from a
directory with section tasks, not from a test directory. For example,
in section 09_functions such a directory structure with tasks:

::

    [~/repos/pyneng-7/pyneng-online-may-aug-2019/exercises/09_functions]
    vagrant: [master|✔]
    $ tree
    .

    ├── config_r1.txt
    ├── config_sw1.txt
    ├── config_sw2.txt
    ├── conftest.py
    ├── task_9_1a.py
    ├── task_9_1.py
    ├── task_9_2a.py
    ├── task_9_2.py
    ├── task_9_3a.py
    ├── task_9_3.py
    ├── task_9_4.py
    └── tests
        ├── test_task_9_1a.py
        ├── test_task_9_1.py
        ├── test_task_9_2a.py
        ├── test_task_9_2.py
        ├── test_task_9_3a.py
        ├── test_task_9_3.py
        └── test_task_9_4.py

In this case, you have to run tests from 09_functions directory:

::

    [~/repos/pyneng-7/pyneng-online-may-aug-2019/exercises/09_functions]
    vagrant: [master|✔]
    $ pytest tests/test_task_9_1.py
    ========================= test session starts ==========================
    platform linux -- Python 3.7.3, pytest-4.6.2, py-1.5.2, pluggy-0.12.0
    rootdir: /home/vagrant/repos/pyneng-7/pyneng-online-may-aug-2019/exercises/09_functions
    collected 3 items

    tests/test_task_9_1.py ...                                       [100%]
    ...

    If you run tests from tests directory, errors will appear.

conftest.py
~~~~~~~~~~~

In addition to test directory there is a conftest.py file - special file in
which you can write functions (more precisely fixtures) common to different
tests. For example, this file contains functions that connect via SSH/Telnet
to equipment.

Useful commands
~~~~~~~~~~~~~~~~

Run one test:

::

    $ pytest tests/test_task_9_1.py

Run one test with more detailed output (shows diff between data in test and
what is received from function):

::

    $ pytest tests/test_task_9_1.py -vv

Start all tests of one section:

::

    [~/repos/pyneng-7/pyneng-online-may-aug-2019/exercises/09_functions]
    vagrant: [master|✔]
    $ pytest
    ======================= test session starts ========================
    platform linux -- Python 3.6.3, pytest-4.6.2, py-1.5.2, pluggy-0.12.0
    rootdir: /home/vagrant/repos/pyneng-7/pyneng-online-may-aug-2019/exercises/09_functions
    collected 21 items

    tests/test_task_9_1.py ..F                                   [ 14%]
    tests/test_task_9_1a.py FFF                                  [ 28%]
    tests/test_task_9_2.py FFF                                   [ 42%]
    tests/test_task_9_2a.py FFF                                  [ 57%]
    tests/test_task_9_3.py FFF                                   [ 71%]
    tests/test_task_9_3a.py FFF                                  [ 85%]
    tests/test_task_9_4.py FFF                                   [100%]

    ...

Starts all tests of the same section with error messages displayed in one line:

::

    $ pytest --tb=line

