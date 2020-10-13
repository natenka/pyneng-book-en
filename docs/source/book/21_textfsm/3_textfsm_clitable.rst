TextFSM CLI Table
-----------------

With TextFSM it is possible to process output of commands and obtain a structured result. However, it is still necessary to manually specify which template will handle show commands each time TextFSM is used.

It would be much more convenient to have some mapping between command and template so that you can write a common script that performs connections to devices, sends commands, chooses template and parse output according to template.

TextFSM has such feature. To use it, you should create a file that describes mapping between commands and templates. In TextFSM it is called **index**.

This file should be in a directory with templates and should have this format:

* first line - column names
* every next line is a pattern match to a command
* mandatory columns with fixed position (mandatory first and last, respectively): 

  * first column - names of templates
  * last column - the corresponding command. This column uses a special format to describe that a command may not be fully written

* other columns are optional 

  * in example below there are columns Hostname, Vendor. They allow you to refine your device information to determine which template to use. For example, *show version* command may be in Cisco and HP devices. Of course, having only commands are not sufficient to determine which template to use. In this case, you can pass information about the type of equipment used with command and then you can define the correct template.

* all columns except the first column support regular expressions. 
  Regular expressions are not supported inside ``[[]]``

Example of index file:

::

    Template, Hostname, Vendor, Command
    sh_cdp_n_det.template, .*, Cisco, sh[[ow]] cdp ne[[ighbors]] de[[tail]]
    sh_clock.template, .*, Cisco, sh[[ow]] clo[[ck]]
    sh_ip_int_br.template, .*, Cisco, sh[[ow]] ip int[[erface]] br[[ief]]
    sh_ip_route_ospf.template, .*, Cisco, sh[[ow]] ip rou[[te]] o[[spf]]

Note how commands are written: ``sh[[ow]] ip int[[erface]] br[[ief]]``. 
Record will be converted to ``sh((ow)?)? ip int((erface)?)? br((ief)?)?``.
This means that TextFSM will be able to determine which template to use even if command is not fully written. For example, such command variants will work:

* sh ip int br 
* show ip inter bri

How to use CLI table
~~~~~~~~~~~~~~~~~~~~~~~~~~

Let’s see how to use *clitable* class and index file.

*templates* directory contains such templates and index file:

::

    sh_cdp_n_det.template
    sh_clock.template
    sh_ip_int_br.template
    sh_ip_route_ospf.template
    index

First we try to work with CLI Table in ipython to see what features this class has and then we look at the final script.

First, we import *clitable* class:

.. code:: python

    In [1]: from textfsm import clitable


.. warning::
    There are different ways to import *clitable* depending on textfsm version:

    * ``import clitable`` for version <= 0.4.1
    * ``from textfsm import clitable`` for version >= 1.1.0

    See textfsm version: ``pip show textfsm``.

We will check *clitable* on the last example from previous section - *show ip route ospf* command. Read the output that is stored in output/sh_ip_route_ospf.txt file to string:

.. code:: python

    In [2]: with open('output/sh_ip_route_ospf.txt') as f:
       ...:     output_sh_ip_route_ospf = f.read()
       ...:


First, you should initialize a class by giving it name of the file in which mapping between templates and commands is stored, and specify name of the directory in which templates are stored:

.. code:: python

    In [3]: cli_table = clitable.CliTable('index', 'templates')

Specify which command should be passed and specify additional attributes that will help to identify template. To do this, you should create a dictionary in which keys are the names of columns that are defined in index file. In this case, it is not necessary to specify vendor name, since *sh ip route ospf* command corresponds to only one template.

.. code:: python

    In [4]: attributes = {'Command': 'show ip route ospf' , 'Vendor': 'Cisco'}

Command output and dictionary with parameters should be passed to ParseCmd method:

.. code:: python

    In [5]: cli_table.ParseCmd(output_sh_ip_route_ospf, attributes)

As a result we have processed output of *sh ip route ospf* command in cli_table object.

cli_table methods (to see all methods, call dir(cli_table)):

.. code:: python

    In [6]: cli_table.
    cli_table.AddColumn        cli_table.NewRow           cli_table.index            cli_table.size
    cli_table.AddKeys          cli_table.ParseCmd         cli_table.index_file       cli_table.sort
    cli_table.Append           cli_table.ReadIndex        cli_table.next             cli_table.superkey
    cli_table.CsvToTable       cli_table.Remove           cli_table.raw              cli_table.synchronised
    cli_table.FormattedTable   cli_table.Reset            cli_table.row              cli_table.table
    cli_table.INDEX            cli_table.RowWith          cli_table.row_class        cli_table.template_dir
    cli_table.KeyValue         cli_table.extend           cli_table.row_index
    cli_table.LabelValueTable  cli_table.header           cli_table.separator

For example, if you call ``print cli_table`` you get this:

.. code:: python

    In [7]: print(cli_table)
    Network, Mask, Distance, Metric, NextHop
    10.0.24.0, /24, 110, 20, ['10.0.12.2']
    10.0.34.0, /24, 110, 20, ['10.0.13.3']
    10.2.2.2, /32, 110, 11, ['10.0.12.2']
    10.3.3.3, /32, 110, 11, ['10.0.13.3']
    10.4.4.4, /32, 110, 21, ['10.0.13.3', '10.0.12.2', '10.0.14.4']
    10.5.35.0, /24, 110, 20, ['10.0.13.3']

FormattedTable method produces a table output:

.. code:: python

    In [8]: print(cli_table.FormattedTable())
     Network    Mask  Distance  Metric  NextHop
    ====================================================================
     10.0.24.0  /24   110       20      10.0.12.2
     10.0.34.0  /24   110       20      10.0.13.3
     10.2.2.2   /32   110       11      10.0.12.2
     10.3.3.3   /32   110       11      10.0.13.3
     10.4.4.4   /32   110       21      10.0.13.3, 10.0.12.2, 10.0.14.4
     10.5.35.0  /24   110       20      10.0.13.3

This can be useful for displaying information.

To get a structured output from cli_table object, such as a list of lists, you have to refer to object in this way:

.. code:: python

    In [9]: data_rows = [list(row) for row in cli_table]

    In [11]: data_rows
    Out[11]:
    [['10.0.24.0', '/24', '110', '20', ['10.0.12.2']],
     ['10.0.34.0', '/24', '110', '20', ['10.0.13.3']],
     ['10.2.2.2', '/32', '110', '11', ['10.0.12.2']],
     ['10.3.3.3', '/32', '110', '11', ['10.0.13.3']],
     ['10.4.4.4', '/32', '110', '21', ['10.0.13.3', '10.0.12.2', '10.0.14.4']],
     ['10.5.35.0', '/24', '110', '20', ['10.0.13.3']]]

You can get column names separately:

.. code:: python

    In [12]: header = list(cli_table.header)

    In [14]: header
    Out[14]: ['Network', 'Mask', 'Distance', 'Metric', 'NextHop']

The output is now similar to that of the previous section.

Assemble everything into one script (textfsm_clitable.py file):

.. code:: python

    import clitable

    output_sh_ip_route_ospf = open('output/sh_ip_route_ospf.txt').read()

    cli_table = clitable.CliTable('index', 'templates')

    attributes = {'Command': 'show ip route ospf' , 'Vendor': 'Cisco'}

    cli_table.ParseCmd(output_sh_ip_route_ospf, attributes)
    print('CLI Table output:\n', cli_table)

    print('Formatted Table:\n', cli_table.FormattedTable())

    data_rows = [list(row) for row in cli_table]
    header = list(cli_table.header)

    print(header)
    for row in data_rows:
        print(row)

In exercises to this section there will be a task to combine described procedure into a function and task to obtain a list of dictionaries.

The output will be:

::

    $ python textfsm_clitable.py
    CLI Table output:
    Network, Mask, Distance, Metric, NextHop
    10.0.24.0, /24, 110, 20, ['10.0.12.2']
    10.0.34.0, /24, 110, 20, ['10.0.13.3']
    10.2.2.2, /32, 110, 11, ['10.0.12.2']
    10.3.3.3, /32, 110, 11, ['10.0.13.3']
    10.4.4.4, /32, 110, 21, ['10.0.13.3', '10.0.12.2', '10.0.14.4']
    10.5.35.0, /24, 110, 20, ['10.0.13.3']

    Formatted Table:
     Network    Mask  Distance  Metric  NextHop
    ====================================================================
     10.0.24.0  /24   110       20      10.0.12.2
     10.0.34.0  /24   110       20      10.0.13.3
     10.2.2.2   /32   110       11      10.0.12.2
     10.3.3.3   /32   110       11      10.0.13.3
     10.4.4.4   /32   110       21      10.0.13.3, 10.0.12.2, 10.0.14.4
     10.5.35.0  /24   110       20      10.0.13.3

    ['Network', 'Mask', 'Distance', 'Metric', 'NextHop']
    ['10.0.24.0', '/24', '110', '20', ['10.0.12.2']]
    ['10.0.34.0', '/24', '110', '20', ['10.0.13.3']]
    ['10.2.2.2', '/32', '110', '11', ['10.0.12.2']]
    ['10.3.3.3', '/32', '110', '11', ['10.0.13.3']]
    ['10.4.4.4', '/32', '110', '21', ['10.0.13.3', '10.0.12.2', '10.0.14.4']]
    ['10.5.35.0', '/24', '110', '20', ['10.0.13.3']]

Now with TextFSM it is possible not only to get a structured output, but also to automatically determine which template to use by command and optional arguments.
