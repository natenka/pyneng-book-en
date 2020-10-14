SQLite
------

`SQLite <http://xgu.ru/wiki/SQLite>`__ — a built-in SQL machine implementation. 
Sqlite is often used as an embedded DBMS in applications.

.. note::

    The word SQL server is not used here because server is not needed there - all functionality that is embedded in SQL server is implemented inside a library (and therefore within program that uses it).


SQLite CLI
^^^^^^^^^^

SQLite package also includes a command line utility for working with SQLite. Utility is presented as a sqlite3 executable file (sqlite3.exe for Windows) and can be used to execute SQL commands manually.

With this utility it is very convenient to check the correctness of SQL commands as well as to get acquainted with SQL language in general.

Let's try to use this utility to figure out basic SQL commands that will be needed to work with database.

We’ll figure out how to build a database first.

.. note::

    If you are using Linux or Mac OS, it is likely that sqlite3 is installed. If you are using Windows you can download sqlite3 `here <http://www.sqlite.org/download.html>`__.

To create a database (or open an already created database), you simply call sqlite3:

::

    $ sqlite3 testDB.db
    SQLite version 3.8.7.1 2014-10-29 13:59:56
    Enter ".help" for usage hints.
    sqlite> 

Inside sqlite3 you can execute SQL commands or so-called metacommands (or dot commands).

Metacommands include several special commands to work with SQLite. They refer only to sqlite3 utility, not to SQL language. There is no need to put ``;`` at the end of command.

Examples of metacommands:

* ``.help`` - a prompt with a list of all metacommands
* ``.exit`` or ``.quit`` - exit sqlite3 session
* ``.databases`` - shows connected databases
* ``.tables`` - shows available tables

Examples of implementation:

::

    sqlite> .help
    .backup ?DB? FILE      Backup DB (default "main") to FILE
    .bail ON|OFF           Stop after hitting an error.  Default OFF
    .databases             List names and files of attached databases
    ...

    sqlite> .databases
    seq  name      file                                   
    ---  --------  ----------------------------------
    0    main      /home/nata/py_for_ne/db/db_article/testDB.db              

litecli
^^^^^^^

The standard Sqlite CLI interface has several disadvantages:

* no autocomplete commands
* no tips
* all content of a column is not always displayed

All these deficiencies are fixed in `litecli <https://github.com/dbcli/litecli>`__.
So it’s best to use it.

Installation of litecli:

::

    $ pip install litecli

Open database in litecli:

::

    $ litecli example.db
    Version: 1.0.0
    Mail: https://groups.google.com/forum/#!forum/litecli-users
    Github: https://github.com/dbcli/litecli
    example.db>

