SELECT
~~~~~~

SELECT operator allows you to query information from the table.

For example:

.. code:: sql

    new_db.db> SELECT * from switch;
    +----------------+----------+------------+-------------------+
    | mac            | hostname | model      | location          |
    +----------------+----------+------------+-------------------+
    | 0010.A1AA.C1CC | sw1      | Cisco 3750 | London, Green Str |
    | 0020.A2AA.C2CC | sw2      | Cisco 3850 | London, Green Str |
    +----------------+----------+------------+-------------------+
    2 rows in set
    Time: 0.033s

``SELECT *`` means that all fields in the table must be displayed. Then indicates from which table data is requested: ``from switch``.

Thus, it is possible to specify specific columns to be derived and in what order:

.. code:: sql

    new_db.db> SELECT hostname, mac, model from switch;
    +----------+----------------+------------+
    | hostname | mac            | model      |
    +----------+----------------+------------+
    | sw1      | 0010.A1AA.C1CC | Cisco 3750 |
    | sw2      | 0020.A2AA.C2CC | Cisco 3850 |
    +----------+----------------+------------+
    2 rows in set
    Time: 0.033s

