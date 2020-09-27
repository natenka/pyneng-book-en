Connection as context manager
---------------------------------

After operations are completed the changes must be saved (apply  ``commit()``), and then you can close connection if it is no longer needed.

Python allows you to use Connection object as a context manager. In that case, you don’t have to explicitly commit.

At the same time:

* If an exception occurs the transaction automatically rolls back 
* if no exception, commit applies automatically

Example of using a database connection as a context manager  (create_sw_inventory_ver2.py):


.. literalinclude:: /pyneng-examples-exercises/examples/18_db/create_sw_inventory_ver2.py
  :language: python
  :linenos:

Note that although a transaction will be rolled back when an exception occurs, the exception itself must still be intercepted.

To check this functionality you should write to the table the data in which MAC address is repeated. But before, in order to not repeat parts of the code, it is better to split the code by functions in create_sw_inventory_ver2.py file (create_sw_inventory_ver2_functions):

.. literalinclude:: /pyneng-examples-exercises/examples/18_db/create_sw_inventory_ver2_functions.py
  :language: python
  :linenos:

The result of script execution is:

::

    $ python create_sw_inventory_ver2_functions.py
    Table creation...
    Data writing to DB:
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
    Data writing was successful
    
    Checking DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]

Now let’s check how write_data_to_db() function will work when there are identical MAC addresses in the data.

File create_sw_inventory_ver3.py uses functions from create_sw_inventory_ver2_functions.py file and implies that the script will run after the previous data is written:

.. literalinclude:: /pyneng-examples-exercises/examples/18_db/create_sw_inventory_ver3.py
  :language: python
  :linenos:

In *data2* list the sw7 switch has the same MAC address as the sw3 switch already existing in database.

Result of script execution:

::

    $ python create_sw_inventory_ver3.py

    Cheking current DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
    ------------------------------------------------------------
    Attempt to write data with repeating MAC address:
    [('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960', 'London, Green Str'),
     ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]
    Error occurred:  UNIQUE constraint failed: switch.mac

    Cheking DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]

Note that the content of *switch* table before and after adding the information is the same. This means that no line from *data2* list has been written.

This is because executemany() method is used and within the same transaction we try to write all four lines. If an error occurs with one of them, all changes are reversed.

Sometimes it’s exactly the kind of behavior you need. If you want to ignore only row with errors you should use execute() method and write each row separately.

File create_sw_inventory_ver4.py has write_rows_to_db() function which writes the data in turn and if there is an error, only changes for specific data are rolled back:

.. literalinclude:: /pyneng-examples-exercises/examples/18_db/create_sw_inventory_ver4.py
  :language: python
  :linenos:

The execution result is (missing only sw7):

::

    $ python create_sw_inventory_ver4.py

    Cheking current DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
    ------------------------------------------------------------
    Attempt to write data with repeating MAC address:
    [('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw7', 'Cisco 2960', 'London, Green Str'),
     ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]
    Data "0055.AAAA.CCCC, sw5, Cisco 3750, London, Green Str" writing was successful
    Data "0066.BBBB.CCCC, sw6, Cisco 3780, London, Green Str" writing was successful
    While writing data "0000.AAAA.DDDD, sw7, Cisco 2960, London, Green Str" the error occured
    Data "0088.AAAA.CCCC, sw8, Cisco 3750, London, Green Str" writing was successful

    Cheking DB content
    [('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
     ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
     ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
     ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str'),
     ('0055.AAAA.CCCC', 'sw5', 'Cisco 3750', 'London, Green Str'),
     ('0066.BBBB.CCCC', 'sw6', 'Cisco 3780', 'London, Green Str'),
     ('0088.AAAA.CCCC', 'sw8', 'Cisco 3750', 'London, Green Str')]

