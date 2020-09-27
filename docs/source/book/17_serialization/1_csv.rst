Work with CSV files
------------------------------

**CSV (comma-separated value)** - a tabular data format (for example, it may be data from a table or data from a database).

In this format, each line of a file is a line of a table. Despite the format name the separator can be not only a comma.

Although formats with a different separator may have their own name such as TSV (tab separated values), CSV is generally understood by all separators.

Example of a CSV file (sw_data.csv):

::

    hostname,vendor,model,location
    sw1,Cisco,3750,London
    sw2,Cisco,3850,Liverpool
    sw3,Cisco,3650,Liverpool
    sw4,Cisco,3650,London

The standard Python library has a csv module that allows working with files in CSV format.

Reading
~~~~~~

Example of reading a file in CSV format (csv_read.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_read.py
  :language: python
  :linenos:


The output is:

::

    $ python csv_read.py
    ['hostname', 'vendor', 'model', 'location']
    ['sw1', 'Cisco', '3750', 'London']
    ['sw2', 'Cisco', '3850', 'Liverpool']
    ['sw3', 'Cisco', '3650', 'Liverpool']
    ['sw4', 'Cisco', '3650', 'London']

First list contains the column names and remaining list contains the corresponding values.

Note that csv.reader returns the iterator:

.. code:: python

    In [1]: import csv

    In [2]: with open('sw_data.csv') as f:
       ...:     reader = csv.reader(f)
       ...:     print(reader)
       ...:
    <_csv.reader object at 0x10385b050>

If necessary it could be converted into a list in the following way:

.. code:: python

    In [3]: with open('sw_data.csv') as f:
       ...:     reader = csv.reader(f)
       ...:     print(list(reader))
       ...:
    [['hostname', 'vendor', 'model', 'location'], ['sw1', 'Cisco', '3750', 'London'], ['sw2', 'Cisco', '3850', 'Liverpool'], ['sw3', 'Cisco', '3650', 'Liverpool'], ['sw4', 'Cisco', '3650', 'London']]

Most often column headers are more convenient to get by a separate object. This can be done in this way (csv_read_headers.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_read_headers.py
  :language: python
  :linenos:


Sometimes it is more convenient to obtain dictionaries in which keys are column names and values are column values.

For this purpose, the module has **DictReader** (csv_read_dict.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_read_dict.py
  :language: python
  :linenos:


The output is:

::

    $ python csv_read_dict.py
    OrderedDict([('hostname', 'sw1'), ('vendor', 'Cisco'), ('model', '3750'), ('location', 'London')])
    sw1 3750
    OrderedDict([('hostname', 'sw2'), ('vendor', 'Cisco'), ('model', '3850'), ('location', 'Liverpool')])
    sw2 3850
    OrderedDict([('hostname', 'sw3'), ('vendor', 'Cisco'), ('model', '3650'), ('location', 'Liverpool')])
    sw3 3650
    OrderedDict([('hostname', 'sw4'), ('vendor', 'Cisco'), ('model', '3650'), ('location', 'London')])
    sw4 3650

Dictreader does not create standard Python dictionaries but ordered dictionaries. Thus, the order of elements corresponds to order of the columns in the CSV file.

.. note::

    Prior to Python 3.6 regular dictionaries were returned, not ordered dictionaries.

Otherwise, it is possible to work with ordered dictionaries using the same methods as in regular dictionaries.

Writing
~~~~~~

Similarly, a csv module can be used to write data to file in CSV format (csv_write.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_write.py
  :language: python
  :linenos:


In the example above, strings from the list are written to the file and then the content of file is displayed on standard output stream.

The output will be as follows:

::

    $ python csv_write.py
    hostname,vendor,model,location
    sw1,Cisco,3750,"London, Best str"
    sw2,Cisco,3850,"Liverpool, Better str"
    sw3,Cisco,3650,"Liverpool, Better str"
    sw4,Cisco,3650,"London, Best str"

Note the interesting thing: strings in the last column are quoted and other values are not.

This is because all strings in the last column have a comma. And the quotation marks indicate what is an entire string. When a comma is inside quotation marks the csv module does not perceive it as a separator.

Sometimes it’s better to have all strings quoted. Of course, in this case, example is simple enough but when there are more values in the strings, the quotation marks indicate where the value begins and ends.

The csv module allows you to control this. For all strings to be written in a CSV file with quotation marks you should change the script this way (csv_write_quoting.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_write_quoting.py
  :language: python
  :linenos:


Now the output is this:

::

    $ python csv_write_quoting.py
    "hostname","vendor","model","location"
    "sw1","Cisco","3750","London, Best str"
    "sw2","Cisco","3850","Liverpool, Better str"
    "sw3","Cisco","3650","Liverpool, Better str"
    "sw4","Cisco","3650","London, Best str"

Now all values are quoted. And because the model number is given as a string in original list, it is quoted here as well.

Besides writerow() method, the writerows() method is supported. It accepts any iterable object.

So, previous example can be written this way (csv_writerows.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_writerows.py
  :language: python
  :linenos:


DictWriter
^^^^^^^^^^

With DictWriter() you can write dictionaries in CSV format.

In general, DictWriter() works as writer() but since dictionaries are not ordered it is necessary to specify the order of columns in file. The *fieldnames* option is used for this purpose (csv_write_dict.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_write_dict.py
  :language: python
  :linenos:


Delimiter
~~~~~~~~~~~~~~~~~~~~

Sometimes other values are used as the separator. In this case, it should be possible to tell the module which separator to use.

For example, if the file uses separator ``;`` (sw_data2.csv file):

::

    hostname;vendor;model;location
    sw1;Cisco;3750;London
    sw2;Cisco;3850;Liverpool
    sw3;Cisco;3650;Liverpool
    sw4;Cisco;3650;London

Simply specify which separator is used in reader() (csv_read_delimiter.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/csv/csv_read_delimiter.py
  :language: python
  :linenos:

