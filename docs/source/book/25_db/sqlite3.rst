.. _sqlite3_index:

Sqlite3 module
--------------

Python uses sqlite3 module to work with SQLite.

``Connection`` object - this object can be said to represent a database.

Example of creating a connection:

.. code:: python

    import sqlite3

    connection = sqlite3.connect('dhcp_snooping.db')

Once you have created a connection you should create a Cursor object which
is the main way to work with database.

Cursor is created from DB connection:

.. code:: python

    connection = sqlite3.connect('dhcp_snooping.db')
    cursor = connection.cursor()

.. toctree::
   :maxdepth: 1

   sqlite3_execute
   sqlite3_fetch
   sqlite3_cursor_as_iterator
   sqlite3_connection_without_cursor
   sqlite3_exception
   sqlite3_context_manager
   example_sqlite
