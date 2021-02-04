Work with YAML files
-------------------------------

**YAML (YAML Ain't Markup Language)** - another text format for writing data.

YAML is more human-friendly than JSON, so it is often used to describe scripts
in software. Ansible, for example.

YAML syntax
~~~~~~~~~~~~~~

Like Python, YAML uses indents to specify the structure of document. But YAML
can only use spaces and cannot use tabs.
Another similarity with Python is that comments start with ``#`` and continue until
the end of line.

List
^^^^^^

A list can be written in one line:

.. code:: yaml

    [switchport mode access, switchport access vlan, switchport nonegotiate, spanning-tree portfast, spanning-tree bpduguard enable]

Or every item in the list in separate row:

.. code:: yaml

    - switchport mode access
    - switchport access vlan
    - switchport nonegotiate
    - spanning-tree portfast
    - spanning-tree bpduguard enable

When a list is written in such a block, each row must start with ``- ``
(minus and space) and all lines in the list must be at the same indentation level.

Dictionary
^^^^^^^

A dictionary can also be written in one line:

.. code:: yaml

    {vlan: 100, name: IT}

Or a block:

.. code:: yaml

    vlan: 100
    name: IT

Strings
^^^^^^

Strings in YAML don't have to be quoted. This is convenient, but sometimes
quotes should be used. For example, when a special character
(special for YAML) is used in a string.

This line, for example, should be quoted to be correctly understood by YAML:

.. code:: yaml

    command: "sh interface | include Queueing strategy:"

Combination of elements
^^^^^^^^^^^^^^^^^^^^

A dictionary with two keys: access and trunk. Values that correspond to
these keys - command lists:

.. code:: yaml

    access:
    - switchport mode access
    - switchport access vlan
    - switchport nonegotiate
    - spanning-tree portfast
    - spanning-tree bpduguard enable

    trunk:
    - switchport trunk encapsulation dot1q
    - switchport mode trunk
    - switchport trunk native vlan 999
    - switchport trunk allowed vlan

List of dictionaries:

.. code:: yaml

    - BS: 1550
      IT: 791
      id: 11
      name: Liverpool
      to_id: 1
      to_name: LONDON
    - BS: 1510
      IT: 793
      id: 12
      name: Bristol
      to_id: 1
      to_name: LONDON
    - BS: 1650
      IT: 892
      id: 14
      name: Coventry
      to_id: 2
      to_name: Manchester

PyYAML module
~~~~~~~~~~~~~

Python uses a PyYAML module to work with YAML. It is not part of the standard
module library, so it needs to be installed:

::

    pip install pyyaml

Work with it is similar to csv and json modules.

Reading from YAML
^^^^^^^^^^^^^^

Converting data from YAML file to Python objects (info.yaml file):

.. code:: yaml

    - BS: 1550
      IT: 791
      id: 11
      name: Liverpool
      to_id: 1
      to_name: LONDON
    - BS: 1510
      IT: 793
      id: 12
      name: Bristol
      to_id: 1
      to_name: LONDON
    - BS: 1650
      IT: 892
      id: 14
      name: Coventry
      to_id: 2
      to_name: Manchester


Reading from YAML (yaml_read.py file):

.. code:: python

    import yaml
    from pprint import pprint

    with open('info.yaml') as f:
        templates = yaml.safe_load(f)

    pprint(templates)

The result is:

::

    $ python yaml_read.py
    [{'BS': 1550,
      'IT': 791,
      'id': 11,
      'name': 'Liverpool',
      'to_id': 1,
      'to_name': 'LONDON'},
     {'BS': 1510,
      'IT': 793,
      'id': 12,
      'name': 'Bristol',
      'to_id': 1,
      'to_name': 'LONDON'},
     {'BS': 1650,
      'IT': 892,
      'id': 14,
      'name': 'Coventry',
      'to_id': 2,
      'to_name': 'Manchester'}]

YAML format is very convenient for storing different parameters, especially
if they are filled manually.

Writing to YAML
^^^^^^^^^^^^^

Write Python objects to YAML (yaml_write.py file):

.. code:: python

    import yaml

    trunk_template = [
        'switchport trunk encapsulation dot1q', 'switchport mode trunk',
        'switchport trunk native vlan 999', 'switchport trunk allowed vlan'
    ]

    access_template = [
        'switchport mode access', 'switchport access vlan',
        'switchport nonegotiate', 'spanning-tree portfast',
        'spanning-tree bpduguard enable'
    ]

    to_yaml = {'trunk': trunk_template, 'access': access_template}

    with open('sw_templates.yaml', 'w') as f:
        yaml.dump(to_yaml, f, default_flow_style=False)

    with open('sw_templates.yaml') as f:
        print(f.read())


File sw_templates.yaml:

.. code:: yaml

    access:
    - switchport mode access
    - switchport access vlan
    - switchport nonegotiate
    - spanning-tree portfast
    - spanning-tree bpduguard enable
    trunk:
    - switchport trunk encapsulation dot1q
    - switchport mode trunk
    - switchport trunk native vlan 999
    - switchport trunk allowed vlan

