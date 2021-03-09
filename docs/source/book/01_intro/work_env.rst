.. raw:: latex

   \newpage

.. _working_env:

Working environment
-------------------

To complete tasks, you can use several options:

-  take a prepared virtual machine vmware or vagrant (virtualbox)
-  prepare a virtual machine yourself
-  use one of the cloud services
-  work without creating a virtual machine

Prepared virtual machines
~~~~~~~~~~~~~~~~~~~~~~~~~

To complete the tasks of the book, virtual machines have been prepared in which
the following are installed:

-  Debian 9.9
-  Python 3.7 and 3.8 in a virtual environment
-  IPython and other modules you will need to complete the tasks
-  text editors vim, Geany, Mu
-  GNS3 for networking equipment

There are two options for virtual machines (the links are for instructions for each option):

-  `Vagrant <https://docs.google.com/document/d/1tIb8prINPM7uhyFxIhSSIF1-jckN_OWkKaO8zHQus9g/edit?usp=sharing>`__. Login/password vagrant/vagrant
-  `VMware <https://drive.google.com/open?id=1r7Si9xTphdWp79sKxDhVk2zjWGggfy5Z6h8cKCLP5Cs>`__. Login/password python/python

Preparing the virtual machine/host yourself
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
* tips for windows :ref:`additional_info_pyneng_windows`
* tips for linux :ref:`additional_info_pyneng_linux`


List of modules to be installed:

::

    pip install pytest pytest-clarity pyyaml tabulate jinja2 textfsm pexpect netmiko graphviz

You also need to install graphviz (example for debian):

::

    apt-get install graphviz

Cloud service
~~~~~~~~~~~~~

Another option is to use one of the following services:
 
-  `repl.it <https://repl.it/>`__ – this service provides an online Python interpreter as well as a graphics editor.
-  `PythonAnywhere <https://www.pythonanywhere.com/>`__ - a separate virtual machine. In the free version you can work only from the command line,
   that is, there is no graphical text editor


