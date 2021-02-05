Book resources
==============


Virtual machine
---------------

To complete tasks, you can use several options:

-  take a prepared virtual machine vmware or vagrant (virtualbox)
-  prepare a virtual machine yourself
-  work without creating a virtual machine

Course VM
~~~~~~~~~

Virtual machines in which the following are installed:

-  Debian 9.9
-  Python 3.7 and 3.8 in a virtual environment
-  IPython and other modules you will need to complete the tasks
-  text editors vim, Geany, Mu
-  GNS3 for networking equipment


There are two options for prepared virtual machines (follow the links for
instructions for each option, which contain links to the image and
instructions on how to work with GNS3):

-  `vagrant <https://docs.google.com/document/d/1tIb8prINPM7uhyFxIhSSIF1-jckN_OWkKaO8zHQus9g/edit?usp=sharing>`__
-  `vmware <https://drive.google.com/open?id=1r7Si9xTphdWp79sKxDhVk2zjWGggfy5Z6h8cKCLP5Cs>`__

Preparing the virtual machine/host yourself
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
List of modules to be installed:

::

    pip install pytest pytest-clarity pyyaml tabulate jinja2 textfsm pexpect netmiko graphviz

You also need to install graphviz (example for debian):

::

    apt-get install graphviz

Exercises
---------

`Repository with examples and tasks <https://github.com/natenka/pyneng-examples-exercises-en/>`__

All tasks and auxiliary files can be downloaded from the repository. Tasks are
duplicated in the book solely for a convenient overview of all tasks in the
section. Since all supporting files, code, and tests are in the repository, 
it is best to do tasks in a copy of the repository. How to make a copy is
described in section 2. Using Git and GitHub.

Sometimes, a certain section can be especially difficult, in this case, you can
stop at a minimum of tasks. This will allow you to move on and not abandon your
studies. Later you can come back and finish the tasks. In general, of course,
it is better to do all the tasks, since practice is the only way to properly
learn the topic, but sometimes it is better to do fewer tasks and continue
studying than to get stuck on one topic and abandon everything.
 

+----------------------------+---------------------------------------+--------------------------------------------------------------+
| Chapter                    | Minimum                               | All tasks                                                    |
+============================+=======================================+==============================================================+
| 04_data_structures         | 4.1, 4.2, 4.3, 4.6                    | 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 4.8                       |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 05_basic_scripts           | 5.1, 5.1a, 5.2, 5.2a                  | 5.1, 5.1a, 5.1b, 5.1c, 5.1d, 5.2, 5.2a, 5.3, 5.3a            |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 06_control_structures      | 6.1, 6.2, 6.3                         | 6.1, 6.2, 6.2a, 6.2b, 6.3                                    |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 07_files                   | 7.1, 7.2, 7.3                         | 7.1, 7.2, 7.2a, 7.2b, 7.2c, 7.3, 7.3a, 7.3b                  |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 09_functions               | 9.1, 9.1a, 9.2, 9.2a, 9.3             | 9.1, 9.1a, 9.2, 9.2a, 9.3, 9.3a, 9.4                         |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 11_modules                 | 11.1, 11.2                            | 11.1, 11.2                                                   |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 12_useful_modules          | 12.1, 12.2                            | 12.1, 12.2, 12.3                                             |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 15_module_re               | 15.1, 15.2, 15.3, 15.4                | 15.1, 15.1a, 15.1b, 15.2, 15.2a, 15.3, 15.4, 15.5            |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 17_serialization           | 17.1, 17.2, 17.3                      | 17.1, 17.2, 17.3, 17.3a, 17.3b, 17.4                         |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 18_ssh_telnet              | 18.1, 18.1a, 18.2, 18.2a, 18.2b, 18.3 | 18.1, 18.1a, 18.1b, 18.2, 18.2a, 18.2b, 18.2c, 18.3          |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 19_concurrent_connections  | 19.1, 19.2, 19.3                      | 19.1, 19.2, 19.3, 19.3a, 19.4                                |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 20_jinja2                  | 20.1, 20.2, 20.3                      | 20.1, 20.2, 20.3, 20.4, 20.5, 20.5a                          |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 21_textfsm                 | 21.1, 21.1a, 21.2, 21.3, 21.4         | 21.1, 21.1a, 21.2, 21.3, 21.4, 21.5                          |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 22_oop_basics              | 22.1, 22.1a, 22.1b, 22.2, 22.2a       | 22.1, 22.1a, 22.1b, 22.1c, 22.1d, 22.2, 22.2a, 22.2b, 22.2c  |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 23_oop_special_methods     | 23.1, 23.1a, 23.2                     | 23.1, 23.1a, 23.2, 23.3, 23.3a                               |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 24_oop_inheritance         | 24.1, 24.2, 24.2a                     | 24.1, 24.1a, 24.2, 24.2a, 24.2b, 24.2c, 24.2d                |
+----------------------------+---------------------------------------+--------------------------------------------------------------+
| 25_db                      | 25.1, 25.2, 25.3                      | 25.1, 25.2, 25.3, 25.4, 25.5, 25.5a, 25.6                    |
+----------------------------+---------------------------------------+--------------------------------------------------------------+


Tests
~~~~~

Starting from section 9. Functions there are automatic tests for checking tasks. 
They help to check whether everything fits the task and also give feedback on
what does not fit the task. As a rule, after first period of adaptation to
tests, it becomes easier to do tasks with tests.

:ref:`How to work with tests and basics of pytest <additional_info_pytest>`. 


Further reading
---------------

Almost every book chapter has subchapter "Further reading" which includes
useful materials and references on the subject, plus references to official
documentations. Moreover, I prepared a
`collection <https://natenka.github.io/pyneng-resources-en/>`__ of
resources on "Python for network engineers" topic where you can find a
lot of useful articles, books, video courses and podcasts.

