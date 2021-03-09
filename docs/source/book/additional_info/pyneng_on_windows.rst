.. raw:: latex

   \newpage

.. _additional_info_pyneng_windows:

Preparing Windows
=================

In order to do tasks on Windows, you need to install Python and Cmder.

Installing Python 3.7
---------------------

Download and install `Python 3.7 <https://www.python.org/downloads/release/python-377/>`__.
Be sure to check the "Add Python 3.7 to PATH" checkbox.

After installation, check:

::

    python --version

The output should be: ``Python 3.7.7``

.. note::

    If you are not going to use the Mu editor, you can install 3.8 as well, but it
    is better to look at Mu first. For the basic topics covered in the course, there
    are practically no changes in 3.8, so you can safely use Python 3.7.


Cmder
-----

Since we work with git on the course, you need to install `Cmder <https://cmder.net/>`__
to work on the course. To do this, select "Download Full".

After installing Cmder, git is immediately available in it and you need
to figure out how it works and configure it to work with `Github <https://pyneng.github.io/docs/git-github-course/>`__.

`You can also make additional Cmder settings <https://medium.com/@alif50/how-to-install-cmder-and-make-it-amazing-c8765e591de5>`__.

Installing Mu
-------------

The only caveat with installing Mu on windows is that it is better to install
it via pip so that the modules that are installed in pip are visible. That is,
you need to install it like this:

::

    pip install mu-editor

Not through the Windows Installer.


Tips for completing tasks on windows
------------------------------------

The pexpect module does not work on Windows, and since it is not needed
to perform tasks, this only affects the fact that it will not be possible
to repeat the examples from the book.

All other modules work, but with some there are nuances.

graphviz
^^^^^^^^

To complete the tasks in sections 11 and 17, you will need graphviz.
And you will need to install the Python module:

::

    pip install graphviz

And app `graphviz <https://graphviz.gitlab.io/_pages/Download/Download_windows.html>`__.

After installation add `graphviz to PATH <https://bobswift.atlassian.net/wiki/spaces/GVIZ/pages/131924165/Graphviz+installation>`__.

csv
^^^

When working with csv on Windows, you always need to specify ``newline =""`` when opening a file:

.. code:: python

    with open(output, "w", newline="") as dest:
        writer = csv.writer(dest)

textfsm
^^^^^^^

Some of the modules that textfsm uses are not available for Windows. And at
the same time, they are not needed for our use of textfsm. For textfsm
to work correctly on Windows, you need to comment out some lines
in the terminal.py file in the textfsm directory.

How to find the directory textfsm.
First, we look at where the site-packages directory is located:

.. code:: python

    In [2]: import sys

    In [3]: sys.path
    Out[3]:
     ...
     'c:\\users\\nata\\appdata\\local\\programs\\python\\python37\\lib\\site-packages',
     ...

Then we go to this directory and inside it we look for the textfsm directory.
In the textfsm directory, open the terminal.py file and comment out the lines in this way:

.. code:: python

    # import fcntl
    import getopt
    import os
    import re
    import struct
    import sys
    # import termios
    import time
    # import tty

After that, the code using textfsm should work on Windows.

Working with network equipment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the last third of the course, you will need network equipment to complete
the assignments. You can use real or virtual network equipment and any equipment
control system: GNS3, UNL or other.

Where to download `GNS3 <https://github.com/GNS3/gns3-gui/releases>`__

Option to connect to virtual devices via Loopback

* right click on start and select Device manager
* press Action, select Add legacy hardware
* Click Next
* select "Install the hardware that i manually select from a list". Click Next
* select Network adapters. Click Next
* select Microsoft in the left column and Microsoft KM-TEST Loopback in the right. Click Next
* click Next
* click Finish
* Be sure to restart the machine afterwards

Then in GNS3 select this loopback interface for connecting the "cloud".
