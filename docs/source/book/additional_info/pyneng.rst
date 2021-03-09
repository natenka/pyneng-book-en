.. raw:: latex

   \newpage

.. _additional_info_pyneng:

Testing tasks with the pyneng utility
=====================================

Starting with "4. Python Data Types", automated tests are used to validate
tasks. They help to check whether everything matches the task, and also
give feedback on what does not correspond to the task. As a rule, after
the first period of adaptation to tests, it becomes easier to do tasks with tests.

In addition to the positive points listed above, in the tests you can also see
what the final result is needed: to clarify the data structure and little
things that can affect the result.

To run the tests, ``pyneng.py`` is used - a script that is located in the
`job repository <https://github.com/natenka/pyneng-examples-exercises-en>`__.

Where to solve tasks
--------------------

Tasks must be performed in prepared files.
For example, section 04_data_structures contains task 4.3. To execute it, open
the exercises/04_data_structures/task_4_3.py file and execute the task directly
in this file after the task description.

This is important because tests are tied to the fact that tasks are executed
in specific files and in a specific directory structure.
In addition to the fact that the tasks must be done in the prepared files,
be sure to copy the entire exercises directory (or even better, the entire
pyneng-examples-exercises-en repository), since tests depend on files in
the exercises directory, not only on files in the specific tasks directory.


Installing the pyneng script
----------------------------

First, you need to install it so that you don't have to write ``python pyneng.py`` every time.

To install the script, the `pyneng.py <https://github.com/natenka/pyneng-examples-exercises-en/blob/master/pyneng.py>`__
and `setup.py <https://github.com/natenka/pyneng-examples-exercises-en/blob/master/setup.py>`__
files must be in the repository. These files are located in the root of the repository.

You need to go to your repository, for example (write the name of your repository):

::

    cd my_repo/

Then, inside the repository, give the command

::

    pip install .

This will install the module and make it possible to call it in any directory
using the word pyneng.


pyneng utility
--------------

Stages of work with tasks:

1. Completion of tasks
2. Checking that the task is working as needed ``python task_4_2.py`` or
   running the script in the editor/IDE
3. Checking assignments with tests ``pyneng 1-5``
4. If the tests pass, look at the solutions ``pyneng 1-5 -a``


.. note::

    The second step is very important because it is much easier to find syntax
    errors and similar problems with the script at this stage than when running
    the code through a test in step 3.

The script makes it easier to run tests, since you do not need to specify any
parameters, by default the output is set to verbose and runs with
the pytest-clarity plugin, which improves diff when the solution is different
from correct result. Also, some things are hidden, for example,
the warning that pytest shows, so as not to distract from the task.

Tests can still be run with :ref:`pytest <additional_info_pytest>` if you are
used to it or have used it before. The pyneng script is just a wrapper around running pytest.

The second part of the script's work is copying the answers to the tasks.
This part is done for convenience, so that you do not have to look for answers
and is made in such a way that first the task must pass the test and only after
that ``pyneng -a`` will work and show the answers (copy them to the current
directory). To copy responses, the script clones the asnwers repository into
the user's home directory, copies the task(s) answer, and deletes
the repository with answers.


Checking tasks with tests
-------------------------

After completing the task, it must be checked using tests. To run the tests,
you need to call pyneng in the task directory. For example, if you are doing
section 4 of the tasks, you need to run pyneng from the
exercises/04_data_structures directory

Testing all tasks of the current section:

::

    pyneng

Running tests for task 4.1:

::

    pyneng 1


Running tests for task 4.1, 4.2, 4.3:

::

    pyneng 1-3

If there are tasks with letters, for example, in section 7, you can run
it in such a way to start checking for tasks 7.2a, 7.2b (you must be in
the 07_files directory):

::

    pyneng 2a-b

or so to run all 7.2x tasks with and without letters:

::

    pyneng 2*

How to get answers
------------------

If the tasks pass the tests, you can see the answers to the tasks.

To do this, add ``-a`` to the previous versions of the command.
Such a call means running tests for tasks 1 and 2 and copying the answers if the tests passed:

::

    pyneng 1-2 -a

For the specified tasks, tests will run, and for those tasks that passed the tests,
the answers will be copied to the answer_task_x.py files in the current directory.


pyneng output
-------------

Warning
^^^^^^^

At the end of the test output, "1 warning" is often written. This can be
ignored, warnings are mainly related to the operation of some modules and
are hidden so as not to distract from the tasks.


Tests passed
^^^^^^^^^^^^

.. figure:: https://raw.githubusercontent.com/pyneng/pyneng.github.io/master/assets/images/ptest_output_5.png

Test failed
^^^^^^^^^^^

When some tests fail, the output shows the difference between what the output
should look like and what output was received.

The differences are shown as Left and Right, unfortunately there is no such
thing that the correct option is highlighted in green, and the wrong one
is highlighted in red, you need to look at the situation. Every time you
display differences, there is a line in front of them like this:

.. code:: python

    assert correct_stdout in out.strip()

In this case, Left is the correct output, right is the output of the task:

.. figure:: https://raw.githubusercontent.com/pyneng/pyneng.github.io/master/assets/images/ptest_output_1.png


another example:

.. code:: python

    return_value == correct_return_value

In this case, Right is the correct output, Left is the output of the task:

.. figure:: https://raw.githubusercontent.com/pyneng/pyneng.github.io/master/assets/images/ptest_output_2.png


