IV. Data writing and transferring
############################

This part of the book deals with data writing and transferring. Data can be, for example:

-  command output
-  processed output of commands as dictionary, list or similar
-  information from monitoring system

So far, only the simplest option has been considered - writing information to a plain text file.

This section deals with data reading and writing in CSV, JSON and YAML formats:

-  CSV - a tabular format of data presentation. It can be obtained, for example, by exporting data from a table or database. Similarly, data can be written in this format for further import into the table.
-  JSON - a format that is often used in API. In addition, this format will allow you to save data structures such as dictionaries or lists in a structured format and then read them from a JSON file and get the same data structures in Python.
-  YAML format is often used to describe scripts. For example, it is used in Ansible. In addition, in this format it is convenient to write manually the parameters that should  be read by scripts.

.. note::

    Python allows the objects of language itself to be written into files and read through the Pickle module, but this aspect is not considered in this book.

Databases are also discussed in this part. Although you can write data to CSV or JSON based on a structure, it is not always convenient to query information from files in this format. This is particularly the case for more complex requests with more than one criterion.

For tasks of this kind, databases are excellent. Section 18 deals with  SQLite as well as the basics of SQL language.

.. toctree::
   :maxdepth: 1

   16_unicode/index.rst
   17_serialization/index.rst
   18_db/index.rst
