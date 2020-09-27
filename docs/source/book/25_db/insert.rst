INSERT
~~~~~~

INSERT operator is used to add data to the table.

.. note::

    If table was deleted in previous step, create it:
    
    .. code:: sql

        new_db.db> create table switch (mac text not NULL primary key, hostname text, model text, location text);
        Query OK, 0 rows affected
        Time: 0.010s

There are several options for adding entries, depending on whether all fields are filled and whether or not they follow the field order.

If values for all fields are specified you can add an entry in this way (the order of fields must be respected):

.. code:: sql

    new_db.db> INSERT into switch values ('0010.A1AA.C1CC', 'sw1', 'Cisco 3750', 'London, Green Str');
    Query OK, 1 row affected
    Time: 0.008s

If you want to specify not all fields or specify them randomly, this entry is used:

.. code:: sql

    new_db.db> INSERT into switch (mac, model, location, hostname) values ('0020.A2AA.C2CC', 'Cisco 3850', 'London, Green Str', 'sw2');
    Query OK, 1 row affected
    Time: 0.009s

