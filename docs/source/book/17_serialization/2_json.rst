Work with JSON files
-------------------------------

**JSON (JavaScript Object Notation)** - a text format for data storage and exchange.

`JSON <https://ru.wikipedia.org/wiki/JSON>`__ syntax is very similar to Python and is user-friendly.

As for CSV, Python has a module that allows easy writing and reading of data in JSON format.


Reading
~~~~~~

File sw_templates.json:


.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/json/sw_templates.json
  :language: json
  :linenos:

There are two methods for reading in json module:

* json.load() - method reads JSON file and returns Python objects
* json.loads() - method reads string in JSON format and returns Python objects

json.load()
^^^^^^^^^^^

Reading JSON file to Python object (json_read_load.py file):


.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/json/json_read_load.py
  :language: python
  :linenos:

The output will be as follows:

.. code:: python

    $ python json_read_load.py
    {'access': ['switchport mode access', 'switchport access vlan', 'switchport nonegotiate', 'spanning-tree portfast', 'spanning-tree bpduguard enable'], 'trunk': ['switchport trunk encapsulation dot1q', 'switchport mode trunk', 'switchport trunk native vlan 999', 'switchport trunk allowed vlan']}
    access
    switchport mode access
    switchport access vlan
    switchport nonegotiate
    spanning-tree portfast
    spanning-tree bpduguard enable
    trunk
    switchport trunk encapsulation dot1q
    switchport mode trunk
    switchport trunk native vlan 999
    switchport trunk allowed vlan

json.loads()
^^^^^^^^^^^^

Reading JSON string to Python object (json_read_loads.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/json/json_read_loads.py
  :language: python
  :linenos:

The result will be similar to previous output.

Writing
~~~~~~

Writing a file in JSON format is also fairly easy.

There are also two methods for writing information in JSON format in json module:

* json.dump() - method writes Python object to file in JSON format
* json.dumps() - method returns string in JSON format

json.dumps()
^^^^^^^^^^^^

Convert object to string in JSON format (json_write_dumps.py):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/json/json_write_dumps.py
  :language: python
  :linenos:


Method json.dumps() is suitable for situations where you want to return a string in JSON format. For example, to pass it to the API.

json.dump()
^^^^^^^^^^^

Write a Python object to a JSON file (json_write_dump.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/json/json_write_dump.py
  :language: python
  :linenos:

When you want to write information in JSON format into a file, it is better to use dump() method.

Additional parameters of write methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Methods dump() and dumps() can pass additional parameters to manage the output format.

By default, these methods write information in a compact view. As a rule, when data is used by other programs, visual presentation of data is not important. If data in file needs to be read  by the person, this format is not very convenient to perceive.

Fortunately, the json module allows you to manage such things.

By passing additional parameters to dump() method (or dumps() method) you can get a more readable output (json_write_indent.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/17_serialization/json/json_write_indent.py
  :language: python
  :linenos:

Now the content of sw_templates.json file is:

::

    {
      "access": [
        "switchport mode access",
        "switchport access vlan",
        "switchport nonegotiate",
        "spanning-tree portfast",
        "spanning-tree bpduguard enable"
      ],
      "trunk": [
        "switchport trunk encapsulation dot1q",
        "switchport mode trunk",
        "switchport trunk native vlan 999",
        "switchport trunk allowed vlan"
      ]
    }

Changing data type
^^^^^^^^^^^^^^^^^^^^^

Another important aspect of data conversion to JSON format is that data will not always be the same type as source data in Python.

For example, when you write a tuple to JSON it becomes a list:

.. code:: python

    In [1]: import json

    In [2]: trunk_template = ('switchport trunk encapsulation dot1q',
       ...:                   'switchport mode trunk',
       ...:                   'switchport trunk native vlan 999',
       ...:                   'switchport trunk allowed vlan')

    In [3]: print(type(trunk_template))
    <class 'tuple'>

    In [4]: with open('trunk_template.json', 'w') as f:
       ...:     json.dump(trunk_template, f, sort_keys=True, indent=2)
       ...:

    In [5]: cat trunk_template.json
    [
      "switchport trunk encapsulation dot1q",
      "switchport mode trunk",
      "switchport trunk native vlan 999",
      "switchport trunk allowed vlan"
    ]
    In [6]: templates = json.load(open('trunk_template.json'))

    In [7]: type(templates)
    Out[7]: list

    In [8]: print(templates)
    ['switchport trunk encapsulation dot1q', 'switchport mode trunk', 'switchport trunk native vlan 999', 'switchport trunk allowed vlan']

This is because JSON uses different data types and does not have matches for all Python data types.

Python data conversion table to JSON:

+---------------+----------+
| Python        | JSON     |
+===============+==========+
| dict          | object   |
+---------------+----------+
| list, tuple   | array    |
+---------------+----------+
| str           | string   |
+---------------+----------+
| int, float    | number   |
+---------------+----------+
| True          | true     |
+---------------+----------+
| False         | false    |
+---------------+----------+
| None          | null     |
+---------------+----------+

JSON conversion table to Python data:

+-----------------+----------+
| JSON            | Python   |
+=================+==========+
| object          | dict     |
+-----------------+----------+
| array           | list     |
+-----------------+----------+
| string          | str      |
+-----------------+----------+
| number (int)    | int      |
+-----------------+----------+
| number (real)   | float    |
+-----------------+----------+
| true            | True     |
+-----------------+----------+
| false           | False    |
+-----------------+----------+
| null            | None     |
+-----------------+----------+

Limitation on data types
^^^^^^^^^^^^^^^^^^^^^^^^^^^

It's not possible to write a dictionary in JSON format if it has tuples as a keys.

.. code:: python

    In [23]: to_json = { ('trunk', 'cisco'): trunk_template, 'access': access_template}

    In [24]: with open('sw_templates.json', 'w') as f:
        ...:     json.dump(to_json, f)
        ...:
    ...
    TypeError: key ('trunk', 'cisco') is not a string

By using additional parameter you can ignore such keys:

.. code:: python

    In [25]: to_json = { ('trunk', 'cisco'): trunk_template, 'access': access_template}

    In [26]: with open('sw_templates.json', 'w') as f:
        ...:     json.dump(to_json, f, skipkeys=True)
        ...:
        ...:

    In [27]: cat sw_templates.json
    {"access": ["switchport mode access", "switchport access vlan", "switchport nonegotiate", "spanning-tree portfast", "spanning-tree bpduguard enable"]}

Beside that, dictionary keys can only be strings in JSON. But if numbers are used in Python dictionary there will be no error. But conversion from numbers to strings will take place:

.. code:: python

    In [28]: d = {1:100, 2:200}

    In [29]: json.dumps(d)
    Out[29]: '{"1": 100, "2": 200}'

