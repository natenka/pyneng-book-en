Package management system Pip
===============================

Pip will be used to install Python packages. It is a package management system used to install packages from the Python Package Index (Pypi). Most likely, if you already have Python installed, pip is installed.

Check pip version:

::

    $ pip --version
    pip 19.1.1 from /home/vagrant/venv/pyneng-py3-7/lib/python3.7/site-packages/pip (python 3.7)


If the command failed, the pip is not installed. Pip installation is described in `documentation <https://pip.pypa.io/en/stable/installing/>`__

Module installation
^^^^^^^^^^^^^^^^^

The command to install modules ``pip install``:

::

    $ pip install tabulate

Removing the package is done as follows:

::

    $ pip uninstall tabulate

In addition, it is sometimes necessary to update the package:

::

    $ pip install --upgrade tabulate

pip or pip3
^^^^^^^^^^^^

Depending on how Python is installed and configured in the system it may be necessary to use pip3 instead of pip. To check which option is used, you must execute the command ``pip --version``.

A variant where pip corresponds to Python 2.7:

::

    $ pip --version
    pip 9.0.1 from /usr/local/lib/python2.7/dist-packages (python 2.7)

A variant where pip3 corresponds to 3.7:

::

    $ pip3 --version
    pip 19.1.1 from /home/vagrant/venv/pyneng-py3-7/lib/python3.7/site-packages/pip (python 3.7)


If the system uses pip3, then every time a Python module is installed in the book it will be necessary to replace pip with pip3.

Alternatively, call pip:

::

    $ python3.7 -m pip install tabulate

Thus, it is always clear for which version of Python the package is installed.
