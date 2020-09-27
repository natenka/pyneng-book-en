CREATE
~~~~~~

CREATE operator allows you to create tables.

First connect to the database or create it with litecli:

::

    $ litecli new_db.db
    Version: 1.0.0
    Mail: https://groups.google.com/forum/#!forum/litecli-users
    Github: https://github.com/dbcli/litecli
    new_db.db>


Create a *switch* table which stores information about switches:

.. code:: sql

    new_db.db> create table switch (mac text not NULL primary key, hostname text, model text, location text);
    Query OK, 0 rows affected
    Time: 0.010s


In this example, we described *switch* table: we defined which fields would be in the table and which types of values would be in them.

Additionally, *mac* field is the primary key. That automatically means that:

* field must be unique
* field cannot have null value (in SQLite this must be stated explicitly)

In this example this is quite logical as MAC address must be unique.

There are no entries in the table at the moment, only a definition. You can view the definition with this command:

.. code:: sql

    new_db.db> .schema switch
    +-----------------------------------------------------------------------------------------------+
    | sql                                                                                           |
    +-----------------------------------------------------------------------------------------------+
    | CREATE TABLE switch (mac text not NULL primary key, hostname text, model text, location text) |
    +-----------------------------------------------------------------------------------------------+
    Time: 0.037s

