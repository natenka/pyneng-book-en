argparse
---------------

``argparse`` is a module for handling command line arguments. Examples of what
a module does:

-  create arguments and options with which script can be called
-  specify argument types, default values
-  indicate which actions correspond to arguments
-  call functions when argument is specified
-  show messages with hints of script usage

``argparse`` is not the only module for handling command line arguments. And
not even the only one in standard library.

This book covers only ``argparse``, but in addition it is worth looking at
modules that are not part of standard Python library. For example,
`click <https://click.palletsprojects.com/>`__.

.. note::
    `A good article <https://realpython.com/blog/python/comparing-python-command-line-parsing-libraries-argparse-docopt-click/>`__,
    compares different command line argument processing modules (covered argparse,
    click and docopt).

Example of ping_function.py script:

.. code:: python

    import subprocess
    import argparse


    def ping_ip(ip_address, count):
        '''
        Ping IP address and return tuple:
        On success: (return code = 0, command output)
        On failure: (return code, error output (stderr))
        '''
        reply = subprocess.run(
            'ping -c {count} -n {ip}'.format(count=count, ip=ip_address),
            shell=True,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            encoding='utf-8'
        )
        if reply.returncode == 0:
            return True, reply.stdout
        else:
            return False, reply.stdout+reply.stderr



    parser = argparse.ArgumentParser(description='Ping script')

    parser.add_argument('-a', action="store", dest="ip")
    parser.add_argument('-c', action="store", dest="count", default=2, type=int)

    args = parser.parse_args()
    print(args)

    rc, message = ping_ip(args.ip, args.count)
    print(message)

Creation of a parser:

* ``parser = argparse.ArgumentParser(description='Ping script')``

Adding arguments:

* ``parser.add_argument('-a', action="store", dest="ip")``

  * rgument that is passed after ``-a`` option is saved to variable ``ip``

* ``parser.add_argument('-c', action="store", dest="count", default=2, type=int)``

  * argument that is passed after ``-c`` option will be saved to variable
    ``count``, but will be converted to a number first. If no argument was
    specified, the default is 2

String ``args = parser.parse_args()`` is specified after all arguments have
been defined. After running it, variable ``args`` contains all arguments
that were passed to the script. They can be accessed using ``args.ip`` syntax.

Let's try a script with different arguments. If both arguments are passed:

::

    $ python ping_function.py -a 8.8.8.8 -c 5
    Namespace(count=5, ip='8.8.8.8')
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: icmp_seq=0 ttl=48 time=48.673 ms
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=48 time=49.902 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=48 time=48.696 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=48 time=50.040 ms
    64 bytes from 8.8.8.8: icmp_seq=4 ttl=48 time=48.831 ms

    --- 8.8.8.8 ping statistics ---
    5 packets transmitted, 5 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 48.673/49.228/50.040/0.610 ms

    Namespace is an object that returns parse\_args() method
 
Pass only IP address:

::

    $ python ping_function.py -a 8.8.8.8
    Namespace(count=2, ip='8.8.8.8')
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: icmp_seq=0 ttl=48 time=48.563 ms
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=48 time=49.616 ms

    --- 8.8.8.8 ping statistics ---
    2 packets transmitted, 2 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 48.563/49.090/49.616/0.526 ms

Call script without arguments:

::

    $ python ping_function.py
    Namespace(count=2, ip=None)
    Traceback (most recent call last):
      File "ping_function.py", line 31, in <module>
        rc, message = ping_ip( args.ip, args.count )
      File "ping_function.py", line 16, in ping_ip
        stderr=temp)
      File "/usr/local/lib/python3.6/subprocess.py", line 336, in check_output
        ````kwargs).stdout
      File "/usr/local/lib/python3.6/subprocess.py", line 403, in run
        with Popen(*popenargs, ````kwargs) as process:
      File "/usr/local/lib/python3.6/subprocess.py", line 707, in __init__
        restore_signals, start_new_session)
      File "/usr/local/lib/python3.6/subprocess.py", line 1260, in _execute_child
        restore_signals, start_new_session, preexec_fn)
    TypeError: expected str, bytes or os.PathLike object, not NoneType

If function was called without arguments when ``argparse`` is not used, an
error would occur that not all arguments are specified.

Because of ``argparse`` the argument is actually passed, but it has ``None`` value.
You can see this in ``Namespace(count=2, ip=None)`` string.

In such a script the IP address must be specified at all times. And in
``argparse`` you can specify that argument is mandatory. To do this,
change ``-a`` option: add ``required=True`` at the end:

.. code:: python

    parser.add_argument('-a', action="store", dest="ip", required=True)

Now, if you call a script without arguments, the output is:

::

    $ python ping_function.py
    usage: ping_function.py [-h] -a IP [-c COUNT]
    ping_function.py: error: the following arguments are required: -a

Now you see a clear message that you need to specify a mandatory argument and
a usage hint.

Also, thanks to ``argparse``, ``help`` is available:

::

    $ python ping_function.py -h
    usage: ping_function.py [-h] -a IP [-c COUNT]

    Ping script

    optional arguments:
      -h, --help  show this help message and exit
      -a IP
      -c COUNT

Note that in message all options are in ``optional arguments`` section.
``argparse`` itself determines that options are specified because they start
with ``-`` and only one letter in name.

Set IP address as a positional argument (ping_function_ver2.py file):

.. code:: python

    import subprocess
    from tempfile import TemporaryFile

    import argparse


    def ping_ip(ip_address, count):
        '''
        Ping IP address and return tuple:
        On success: (return code = 0, command output)
        On failure: (return code, error output (stderr))
        '''
        reply = subprocess.run(
            'ping -c {count} -n {ip}' .format(count=count, ip=ip_address),
            shell=True,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            encoding='utf-8',
        )
        if reply.returncode == 0:
            return True, reply.stdout
        else:
            return False, reply.stdout+reply.stderr



    parser = argparse.ArgumentParser(description='Ping script')

    parser.add_argument('host', action="store", help="IP or name to ping")
    parser.add_argument('-c', action="store", dest="count", default=2, type=int,
                        help="Number of packets")

    args = parser.parse_args()
    print(args)

    rc, message = ping_ip( args.host, args.count )
    print(message)

Now instead of giving ``-a`` option you can simply pass IP address. 
It will be automatically saved in ``host`` variable.
And it's automatically considered as a mandatory. Тhat is, it is no longer
necessary to specify ``required=True`` and ``dest="ip"``.

In addition, script specifies messages that will be displayed when
you call ``help``. Now script call looks like this:

::

    $ python ping_function_ver2.py 8.8.8.8 -c 2
    Namespace(host='8.8.8.8', count=2)
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: icmp_seq=0 ttl=48 time=49.203 ms
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=48 time=51.764 ms

    --- 8.8.8.8 ping statistics ---
    2 packets transmitted, 2 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 49.203/50.484/51.764/1.280 ms

``help`` message:

::

    $ python ping_function_ver2.py -h
    usage: ping_function_ver2.py [-h] [-c COUNT] host

    Ping script

    positional arguments:
      host        IP or name to ping

    optional arguments:
      -h, --help  show this help message and exit
      -c COUNT    Number of packets

Nested parsers
~~~~~~~~~~~~~~~~~

Consider one of the methods to organize a more complex hierarchy of arguments.

.. note::
    This example will show more features of ``argparse`` but they are not
    limited to that, so if you use ``argparse`` you should check
    `module documentation <https://docs.python.org/3/library/argparse.html>`__ or
    `article on PyMOTW <https://pymotw.com/3/argparse/>`__.

File parse_dhcp_snooping.py:

.. code:: python

    # -*- coding: utf-8 -*-
    import argparse

    # Default values:
    DFLT_DB_NAME = 'dhcp_snooping.db'
    DFLT_DB_SCHEMA = 'dhcp_snooping_schema.sql'


    def create(args):
        print("Creating DB {} with DB schema {}".format((args.name, args.schema)))


    def add(args):
        if args.sw_true:
            print("Adding switch data to database")
        else:
            print("Reading info from file(s) \n{}".format(', '.join(args.filename)))
            print("\nAdding data to db {}".format(args.db_file))


    def get(args):
        if args.key and args.value:
            print("Geting data from DB: {}".format(args.db_file))
            print("Request data for host(s) with {} {}".format((args.key, args.value)))
        elif args.key or args.value:
            print("Please give two or zero args\n")
            print(show_subparser_help('get'))
        else:
            print("Showing {} content...".format(args.db_file))


    parser = argparse.ArgumentParser()
    subparsers = parser.add_subparsers(title='subcommands',
                                       description='valid subcommands',
                                       help='description')


    create_parser = subparsers.add_parser('create_db', help='create new db')
    create_parser.add_argument('-n', metavar='db-filename', dest='name',
                               default=DFLT_DB_NAME, help='db filename')
    create_parser.add_argument('-s', dest='schema', default=DFLT_DB_SCHEMA,
                               help='db schema filename')
    create_parser.set_defaults(func=create)


    add_parser = subparsers.add_parser('add', help='add data to db')
    add_parser.add_argument('filename', nargs='+', help='file(s) to add to db')
    add_parser.add_argument('--db', dest='db_file', default=DFLT_DB_NAME, help='db name')
    add_parser.add_argument('-s', dest='sw_true', action='store_true',
                            help='add switch data if set, else add normal data')
    add_parser.set_defaults(func=add)


    get_parser = subparsers.add_parser('get', help='get data from db')
    get_parser.add_argument('--db', dest='db_file', default=DFLT_DB_NAME, help='db name')
    get_parser.add_argument('-k', dest="key",
                            choices=['mac', 'ip', 'vlan', 'interface', 'switch'],
                            help='host key (parameter) to search')
    get_parser.add_argument('-v', dest="value", help='value of key')
    get_parser.add_argument('-a', action='store_true', help='show db content')
    get_parser.set_defaults(func=get)



    if __name__ == '__main__':
        args = parser.parse_args()
        if not vars(args):
            parser.print_usage()
        else:
            args.func(args)

Now not only a parser is created as in previous example, but also nested parsers.
Nested parsers will be displayed as commands. In fact, they will be used as
mandatory arguments.

With help of nested parsers a hierarchy of arguments and options is created.
Arguments that are added to nested parser will be available as arguments for
this parser. For example, this part creates a nested ``create_db`` parser and
adds ``-n`` option:

.. code:: python

    create_parser = subparsers.add_parser('create_db', help='create new db')
    create_parser.add_argument('-n', dest='name', default=DFLT_DB_NAME,
                               help='db filename')

Syntax for creating nested parsers and adding arguments to them is the same:

.. code:: python

    create_parser = subparsers.add_parser('create_db', help='create new db')
    create_parser.add_argument('-n', metavar='db-filename', dest='name',
                               default=DFLT_DB_NAME, help='db filename')
    create_parser.add_argument('-s', dest='schema', default=DFLT_DB_SCHEMA,
                               help='db schema filename')
    create_parser.set_defaults(func=create)

Method ``add_argument`` adds an argument. Here, syntax is exactly the same as
without nested parsers.

String ``create_parser.set_defaults(func=create)`` specifies that the ``create``
function will be called when calling the ``create_parser`` parser.

Function ``create`` receives as an argument all arguments that have been
passed. And within function you can access to necessary arguments:

.. code:: python

    def create(args):
        print("Creating DB {} with DB schema {}".format(args.name, args.schema))

If you call ``help`` for this script, the output is:

::

    $ python parse_dhcp_snooping.py -h
    usage: parse_dhcp_snooping.py [-h] {create_db,add,get} ...

    optional arguments:
      -h, --help           show this help message and exit

    subcommands:
      valid subcommands

      {create_db,add,get}  description
        create_db          create new db
        add                add data to db
        get                get data from db

Note that each nested parser that is created in the script is displayed as a
command in usage hint:

::

    usage: parse_dhcp_snooping.py [-h] {create_db,add,get} ...

Each nested parser now has its own ``help``:

::

    $ python parse_dhcp_snooping.py create_db -h
    usage: parse_dhcp_snooping.py create_db [-h] [-n db-filename] [-s SCHEMA]

    optional arguments:
      -h, --help      show this help message and exit
      -n db-filename  db filename
      -s SCHEMA       db schema filename

In addition to nested parsers, there are also several new features
of ``argparse`` in this example.

``metavar``
^^^^^^^^^^^

Parser ``create_parser`` uses a new argument - ``metavar``:

.. code:: python

    create_parser.add_argument('-n', metavar='db-filename', dest='name',
                               default=DFLT_DB_NAME, help='db filename')
    create_parser.add_argument('-s', dest='schema', default=DFLT_DB_SCHEMA,
                               help='db schema filename')

Argument ``metavar`` allows you to specify argument name to show it in ``usage``
message and ``help``:

::

    $ python parse_dhcp_snooping.py create_db -h
    usage: parse_dhcp_snooping.py create_db [-h] [-n db-filename] [-s SCHEMA]

    optional arguments:
      -h, --help      show this help message and exit
      -n db-filename  db filename
      -s SCHEMA       db schema filename

Look at the difference between ``-n`` and ``-s`` options:

-  after ``-n`` option in both ``usage`` and ``help`` the name is specified
   in the ``metavar`` parameter 
-  after ``-s`` option the name is specified to which value is saved

``nargs``
^^^^^^^^^

Parser ``add_parser`` uses ``nargs``:

.. code:: python

    add_parser.add_argument('filename', nargs='+', help='file(s) to add to db')

Parameter ``nargs`` allows to specify a certain number of elements that must
be entered into this argument. In this case, all arguments that have been passed
to the script after ``filename`` argument will be included in ``nargs`` list,
but at least one argument must be passed.

In this case, ``help`` message looks like:

::

    $ python parse_dhcp_snooping.py add -h
    usage: parse_dhcp_snooping.py add [-h] [--db DB_FILE] [-s]
                                      filename [filename ...]

    positional arguments:
      filename      file(s) to add to db

    optional arguments:
      -h, --help    show this help message and exit
      --db DB_FILE  db name
      -s            add switch data if set, else add normal data

If you pass several files, they'll be in the list. And since ``add`` function
simply displays file names, the output is:

::

    $ python parse_dhcp_snooping.py add filename test1.txt test2.txt
    Reading info from file(s)
    filename, test1.txt, test2.txt

    Adding data to db dhcp_snooping.db

``nargs`` supports such values as:

-  ``N`` - - number of arguments should be specified. Arguments will be in list (even if only one is specified)
-  ``?`` - 0 or 1 argument
-  ``*`` - all arguments will be in list
-  ``+`` - all arguments will be in list, but at least one argument has to be passed

``choices``
^^^^^^^^^^^

Parser ``get_parser`` uses ``choices``:

.. code:: python

    get_parser.add_argument('-k', dest="key",
                            choices=['mac', 'ip', 'vlan', 'interface', 'switch'],
                            help='host key (parameter) to search')

For some arguments it is important that the value is selected only from certain
options. In such cases you can specify ``choices``.

For this parser ``help`` looks like this:

::

    $ python parse_dhcp_snooping.py get -h
    usage: parse_dhcp_snooping.py get [-h] [--db DB_FILE]
                                      [-k {mac,ip,vlan,interface,switch}]
                                      [-v VALUE] [-a]

    optional arguments:
      -h, --help            show this help message and exit
      --db DB_FILE          db name
      -k {mac,ip,vlan,interface,switch}
                            host key (parameter) to search
      -v VALUE              value of key
      -a                    show db content

And if you choose the wrong option:

::

    $ python parse_dhcp_snooping.py get -k test
    usage: parse_dhcp_snooping.py get [-h] [--db DB_FILE]
                                      [-k {mac,ip,vlan,interface,switch}]
                                      [-v VALUE] [-a]
    parse_dhcp_snooping.py get: error: argument -k: invalid choice: 'test' (choose from 'mac', 'ip', 'vlan', 'interface', 'switch')


In this example it is important to specify allowed options that could be chosen
because based on chosen option the SQL-query is generated. And thanks to
``choices`` there is no pissibility to specify parameter that is not allowed.

Parser import
^^^^^^^^^^^^^^

In parse_dhcp_snooping.py, the last two lines will only be executed if script
has been called as a main script.

.. code:: python

    if __name__ == '__main__':
        args = parser.parse_args()
        args.func(args)

Therefore, if you import a file these lines will not be called.

Trying to import parser into another file (call_pds.py file):

.. code:: python

    from parse_dhcp_snooping import parser

    args = parser.parse_args()
    args.func(args)

Call ``help`` message:

::

    $ python call_pds.py -h
    usage: call_pds.py [-h] {create_db,add,get} ...

    optional arguments:
      -h, --help           show this help message and exit

    subcommands:
      valid subcommands

      {create_db,add,get}  description
        create_db          create new db
        add                add data to db
        get                get data from db

Invoking the argument:

::

    $ python call_pds.py add test.txt test2.txt
    Reading info from file(s)
    test.txt, test2.txt

    Adding data to db dhcp_snooping.db

Everything works without a problem.

Passing of arguments manually
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The last feature of ``argparse`` is the ability to pass arguments manually.

Arguments can be passed as a list when calling ``parse_args`` method
(call_pds2.py file):

.. code:: python

    from parse_dhcp_snooping import parser, get

    args = parser.parse_args('add test.txt test2.txt'.split())
    args.func(args)

It is necessary to use ``split`` method since ``parse_args`` method
expects list of arguments.

The result will be the same as if script was called with arguments:

::

    $ python call_pds2.py
    Reading info from file(s)
    test.txt, test2.txt

    Adding data to db dhcp_snooping.db

