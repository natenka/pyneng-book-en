Tabulate
---------------

**tabulate** is a module that allows you to display table data beautifully. It is not part of standard Python library, so **tabulate** needs to be installed:

::

    pip install tabulate

Module supports such tabular data types as:

* list of lists (in general case - iterable of iterables)
* dictionary list (or any other iterable object with dictionaries). Keys are used as column names
* dictionary with iterable objects. Keys are used as column names

Function tabulate() is used to generate table:

.. code:: python

    In [1]: from tabulate import tabulate

    In [2]: sh_ip_int_br = [('FastEthernet0/0', '15.0.15.1', 'up', 'up'),
       ...:  ('FastEthernet0/1', '10.0.12.1', 'up', 'up'),
       ...:  ('FastEthernet0/2', '10.0.13.1', 'up', 'up'),
       ...:  ('Loopback0', '10.1.1.1', 'up', 'up'),
       ...:  ('Loopback100', '100.0.0.1', 'up', 'up')]
       ...:

    In [4]: print(tabulate(sh_ip_int_br))
    ---------------  ---------  --  --
    FastEthernet0/0  15.0.15.1  up  up
    FastEthernet0/1  10.0.12.1  up  up
    FastEthernet0/2  10.0.13.1  up  up
    Loopback0        10.1.1.1   up  up
    Loopback100      100.0.0.1  up  up
    ---------------  ---------  --  --

headers
~~~~~~~

Parameter **headers** allows you to pass an additional argument that specifies column names:

.. code:: python

    In [8]: columns=['Interface', 'IP', 'Status', 'Protocol']

    In [9]: print(tabulate(sh_ip_int_br, headers=columns))
    Interface        IP         Status    Protocol
    ---------------  ---------  --------  ----------
    FastEthernet0/0  15.0.15.1  up        up
    FastEthernet0/1  10.0.12.1  up        up
    FastEthernet0/2  10.0.13.1  up        up
    Loopback0        10.1.1.1   up        up
    Loopback100      100.0.0.1  up        up

Quite often, the first data set is headers. Then it is enough to specify headers equal to "firstrow":

.. code:: python

    In [18]: data
    Out[18]:
    [('Interface', 'IP', 'Status', 'Protocol'),
     ('FastEthernet0/0', '15.0.15.1', 'up', 'up'),
     ('FastEthernet0/1', '10.0.12.1', 'up', 'up'),
     ('FastEthernet0/2', '10.0.13.1', 'up', 'up'),
     ('Loopback0', '10.1.1.1', 'up', 'up'),
     ('Loopback100', '100.0.0.1', 'up', 'up')]

    In [20]: print(tabulate(data, headers='firstrow'))
    Interface        IP         Status    Protocol
    ---------------  ---------  --------  ----------
    FastEthernet0/0  15.0.15.1  up        up
    FastEthernet0/1  10.0.12.1  up        up
    FastEthernet0/2  10.0.13.1  up        up
    Loopback0        10.1.1.1   up        up
    Loopback100      100.0.0.1  up        up

If data is in the form of a list of dictionaries, you should specify headers equal to "keys":

.. code:: python

    In [22]: list_of_dict
    Out[22]:
    [{'IP': '15.0.15.1',
      'Interface': 'FastEthernet0/0',
      'Protocol': 'up',
      'Status': 'up'},
     {'IP': '10.0.12.1',
      'Interface': 'FastEthernet0/1',
      'Protocol': 'up',
      'Status': 'up'},
     {'IP': '10.0.13.1',
      'Interface': 'FastEthernet0/2',
      'Protocol': 'up',
      'Status': 'up'},
     {'IP': '10.1.1.1',
      'Interface': 'Loopback0',
      'Protocol': 'up',
      'Status': 'up'},
     {'IP': '100.0.0.1',
      'Interface': 'Loopback100',
      'Protocol': 'up',
      'Status': 'up'}]

    In [23]: print(tabulate(list_of_dict, headers='keys'))
    Interface        IP         Status    Protocol
    ---------------  ---------  --------  ----------
    FastEthernet0/0  15.0.15.1  up        up
    FastEthernet0/1  10.0.12.1  up        up
    FastEthernet0/2  10.0.13.1  up        up
    Loopback0        10.1.1.1   up        up
    Loopback100      100.0.0.1  up        up

Table style
~~~~~~~~~~~~~

**tabulate** supports different table display styles.

Table in Grid format:

::

    In [24]: print(tabulate(list_of_dict, headers='keys', tablefmt="grid"))
    +-----------------+-----------+----------+------------+
    | Interface       | IP        | Status   | Protocol   |
    +=================+===========+==========+============+
    | FastEthernet0/0 | 15.0.15.1 | up       | up         |
    +-----------------+-----------+----------+------------+
    | FastEthernet0/1 | 10.0.12.1 | up       | up         |
    +-----------------+-----------+----------+------------+
    | FastEthernet0/2 | 10.0.13.1 | up       | up         |
    +-----------------+-----------+----------+------------+
    | Loopback0       | 10.1.1.1  | up       | up         |
    +-----------------+-----------+----------+------------+
    | Loopback100     | 100.0.0.1 | up       | up         |
    +-----------------+-----------+----------+------------+

Table in Markdown format:

::

    In [25]: print(tabulate(list_of_dict, headers='keys', tablefmt='pipe'))
    | Interface       | IP        | Status   | Protocol   |
    |:----------------|:----------|:---------|:-----------|
    | FastEthernet0/0 | 15.0.15.1 | up       | up         |
    | FastEthernet0/1 | 10.0.12.1 | up       | up         |
    | FastEthernet0/2 | 10.0.13.1 | up       | up         |
    | Loopback0       | 10.1.1.1  | up       | up         |
    | Loopback100     | 100.0.0.1 | up       | up         |

Table in HTML format:

::

    In [26]: print(tabulate(list_of_dict, headers='keys', tablefmt='html'))
    <table>
    <thead>
    <tr><th>Interface      </th><th>IP       </th><th>Status  </th><th>Protocol  </th></tr>
    </thead>
    <tbody>
    <tr><td>FastEthernet0/0</td><td>15.0.15.1</td><td>up      </td><td>up        </td></tr>
    <tr><td>FastEthernet0/1</td><td>10.0.12.1</td><td>up      </td><td>up        </td></tr>
    <tr><td>FastEthernet0/2</td><td>10.0.13.1</td><td>up      </td><td>up        </td></tr>
    <tr><td>Loopback0      </td><td>10.1.1.1 </td><td>up      </td><td>up        </td></tr>
    <tr><td>Loopback100    </td><td>100.0.0.1</td><td>up      </td><td>up        </td></tr>
    </tbody>
    </table>

Alignment of columns
~~~~~~~~~~~~~~~~~~~~~

You can specify alignment for columns:

.. code:: python

    In [27]: print(tabulate(list_of_dict, headers='keys', tablefmt='pipe', stralign='center'))
    |    Interface    |    IP     |  Status  |  Protocol  |
    |:---------------:|:---------:|:--------:|:----------:|
    | FastEthernet0/0 | 15.0.15.1 |    up    |     up     |
    | FastEthernet0/1 | 10.0.12.1 |    up    |     up     |
    | FastEthernet0/2 | 10.0.13.1 |    up    |     up     |
    |    Loopback0    | 10.1.1.1  |    up    |     up     |
    |   Loopback100   | 100.0.0.1 |    up    |     up     |

Note that not only columns are displayed centrally, but Markdown syntax has been changed accordingly.

Additional material
~~~~~~~~~~~~~~~~~~~~~~~~

-  `tabulate documentation <https://bitbucket.org/astanin/python-tabulate>`__

Articles from author **tabulate**:

* `Pretty printing tables in Python <https://txt.arboreus.com/2013/03/13/pretty-print-tables-in-python.html>`__
* `Tabulate 0.7.1 with LaTeX & MediaWiki tables <https://txt.arboreus.com/2013/12/12/tabulate-0-7-1-with-latex-tables-named-tuples-etc.html>`__

Stack Overflow:

* `Printing Lists as Tabular Data <https://stackoverflow.com/questions/9535954/printing-lists-as-tabular-data>`__.
  Note `the answer <https://stackoverflow.com/a/26937531>`__ - it contains other tabulate analogues.
