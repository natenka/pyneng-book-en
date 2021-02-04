Virtual environment
=====================

Virtual environments:

-  Allow different projects to be isolated from each other
-  Packages that are needed by different projects are in different places - for example, if one project requires 1.0 package and another project requires the same package but version 3.1, they will not interfere with each other
-  Packages that are installed in virtual environments do not impact on global packages

.. note::
    Python has several options for creating virtual environments. You can use any of them. To start with, you can use virtualenvwrapper and then eventually you can figure out which options are still available.


virtualenvwrapper
^^^^^^^^^^^^^^^^^

Virtual environments are created with virtualenvwrapper.

Installing virtualenvwrapper with pip:

::

    $ sudo pip3.7 install virtualenvwrapper

After installation, in . bashrc file in current user’s home folder, you need to add several lines:

::

    export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3.7
    export WORKON_HOME=~/venv
    . /usr/local/bin/virtualenvwrapper.sh

If you are using a command interpreter other than bash, see if it is supported in virtualenvwrapper 
`documentation <http://virtualenvwrapper.readthedocs.io/en/latest/install.html>`__. Environment variable VIRTUALENVWRAPPER\_PYTHON
points to Python command line binary file, WORKON\_HOME – points to location of virtual environments. The third line indicates location of script installed with virtualenvwrapper package. To start virtualenvwrapper.sh script work with virtual environments, bash must be restarted.

Restart command interpreter:

::

    $ exec bash

This may not always be the right option. More on `Stack
Overflow <http://stackoverflow.com/questions/2518127/how-do-i-reload-bashrc-without-logging-out-and-back-in>`__.

Working with virtual environments
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Creating a new virtual environment in which Python 3.7 is used by default:

::

    $ mkvirtualenv --python=/usr/local/bin/python3.7 pyneng
    New python executable in PyNEng/bin/python
    Installing distribute........................done.
    Installing pip...............done.
    (pyneng)$ 

The name of virtual environment is shown in parentheses before standard invitation.
That means you’re inside it. Virtualenvwrapper uses Tab to autocomplete name of
virtual environment. This is particularly useful when there are many virtual
environments. Now “pyneng” directory was created in directory specified in
environment variable WORKON_HOME:

::

    (pyneng)$ ls -ls venv
    total 52
    ....
    4 -rwxr-xr-x 1 nata nata   99 Sep 30 16:41 preactivate
    4 -rw-r--r-- 1 nata nata   76 Sep 30 16:41 predeactivate
    4 -rwxr-xr-x 1 nata nata   91 Sep 30 16:41 premkproject
    4 -rwxr-xr-x 1 nata nata  130 Sep 30 16:41 premkvirtualenv
    4 -rwxr-xr-x 1 nata nata  111 Sep 30 16:41 prermvirtualenv
    4 drwxr-xr-x 6 nata nata 4096 Sep 30 16:42 pyneng

Exit virtual environment:

::

    (pyneng)$ deactivate 
    $ 

To move to created virtual environment, you should run "workon" command:

::

    $ workon pyneng
    (pyneng)$ 

If you want to go from one virtual environment to another, you don’t need to do deactivate, you can go directly through "workon":

::

    $ workon Test
    (Test)$ workon pyneng
    (pyneng)$ 

If you want to remove virtual environment, you should use "rmvirtualenv":

::

    $ rmvirtualenv Test
    Removing Test...
    $ 

See which packages are installed in a virtual environment using "lssitepackages":

::

    (pyneng)$ lssitepackages
    ANSI.py                                pexpect-3.3-py2.7.egg-info
    ANSI.pyc                               pickleshare-0.5-py2.7.egg-info
    decorator-4.0.4-py2.7.egg-info         pickleshare.py
    decorator.py                           pickleshare.pyc
    decorator.pyc                          pip-1.1-py2.7.egg
    distribute-0.6.24-py2.7.egg            pxssh.py
    easy-install.pth                       pxssh.pyc
    fdpexpect.py                           requests
    fdpexpect.pyc                          requests-2.7.0-py2.7.egg-info
    FSM.py                                 screen.py
    FSM.pyc                                screen.pyc
    IPython                                setuptools.pth
    ipython-4.0.0-py2.7.egg-info           simplegeneric-0.8.1-py2.7.egg-info
    ipython_genutils                       simplegeneric.py
    ipython_genutils-0.1.0-py2.7.egg-info  simplegeneric.pyc
    path.py                                test_path.py
    path.py-8.1.1-py2.7.egg-info           test_path.pyc
    path.pyc                               traitlets
    pexpect                                traitlets-4.0.0-py2.7.egg-info

Built-in venv module
^^^^^^^^^^^^^^^^^^^^^^

Starting from version 3.5, it is recommended that Python use venv to create virtual environments:

::

    $ python3.7 -m venv new/pyneng

Python or python3 can be used instead of python 3.7, depending on how Python 3.7 is installed. This command creates specified directory and all necessary subdirectories within it if they have not been created.

Command creates the following directory structure:

::

    $ ls -ls new/pyneng
    total 16
    4 drwxr-xr-x 2 vagrant vagrant 4096 Aug 21 14:50 bin
    4 drwxr-xr-x 2 vagrant vagrant 4096 Aug 21 14:50 include
    4 drwxr-xr-x 3 vagrant vagrant 4096 Aug 21 14:50 lib
    4 -rw-r--r-- 1 vagrant vagrant   75 Aug 21 14:50 pyvenv.cfg

To move to a virtual environment, you should execute command:

::

    $ source new/pyneng/bin/activate

To exit virtual environment, use command “deactivate”:

::

    $ deactivate

More about the venv module in
`documentation <https://docs.python.org/3/library/venv.html#module-venv>`__.

Package installation
^^^^^^^^^^^^^^^^^

For example, let's install simplejson package in virtual environment.

::

    (pyneng)$ pip install simplejson
    ...
    Successfully installed simplejson
    Cleaning up...

If you open Python interpreter and import simplejson, it is available and there are no errors:

::

    (pyneng)$ python
    >>> import simplejson
    >>> simplejson
    <module 'simplejson' from '/home/vagrant/venv/pyneng-py3-7/lib/python3.7/site-packages/simplejson/__init__.py'>
    >>>

But if you exit from virtual environment and try to do the same thing, there is no such module:

::

    (pyneng)$ deactivate 

    $ python
    >>> import simplejson
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ModuleNotFoundError: No module named 'simplejson'
    >>> 

