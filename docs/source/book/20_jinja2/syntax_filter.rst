Filters
-------

In Jinja, variables can be changed by filters. Filters are separated from
variable by a vertical line (pipe ``|``) and may contain additional arguments.
In addition, several filters can be applied to variable. In this case,
filters are simply written consecutively and each of them is separated by a vertical line.

Jinja supports a large number of built-in filters. We will look at only a few
of them. Other filters can be found in `documentation <http://jinja.pocoo.org/docs/dev/templates/#builtin-filters>`__.

You can also easily create your own filters. We will not cover this
possibility but it is `well documented <http://jinja.pocoo.org/docs/2.9/api/#custom-filters>`__.

default
~~~~~~~

Filter ``default`` allows you to set default value for variable. If variable
is defined, it will be displayed, if variable is not defined, the value
specified in default filter will be displayed.

Template example templates/filter_default.txt:

::

    router ospf 1
     auto-cost reference-bandwidth {{ ref_bw | default(10000) }}
     {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
     {% endfor %}

If variable ref_bw is defined in dictionary, its value will be set. If there is no variable,
the value of 10000 will be substituted.

Data file (data_files/filter_default.yml):

.. code:: yaml

    ospf:
      - network: 10.0.1.0 0.0.0.255
        area: 0
      - network: 10.0.2.0 0.0.0.255
        area: 2
      - network: 10.1.1.0 0.0.0.255
        area: 0

The result of execution:

::

    $ python cfg_gen.py templates/filter_default.txt data_files/filter_default.yml
    router ospf 1
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

By default, if variable is defined and its value is empty, it will be assumed
that variable and its value exist.

If you want default value to be set also when variable is empty
(i.e., treated as False in Python), you need to specify additional
parameter ``boolean=true``.

For example, if data file is:

::

    ref_bw: ''
    ospf:
      - network: 10.0.1.0 0.0.0.255
        area: 0
      - network: 10.0.2.0 0.0.0.255
        area: 2
      - network: 10.1.1.0 0.0.0.255
        area: 0

The result will be:

::

    $ python cfg_gen.py templates/filter_default.txt data_files/filter_default.yml
    router ospf 1
     auto-cost reference-bandwidth 
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

If with the same data file the template will be changed as follows:

::

    router ospf 1
     auto-cost reference-bandwidth {{ ref_bw | default(10000, boolean=true) }}
    {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
    {% endfor %}

.. note::
    Instead of ``default(10000, boolean=true)`` you can write
    default(10000, true)

The result will be (default value is set):

::

    $ python cfg_gen.py templates/filter_default.txt data_files/filter_default.yml
    router ospf 1
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

dictsort
~~~~~~~~

Filter ``dictsort`` allows you to sort the dictionary. By default, sorting is
done by keys but by changing filter parameters you can sort by values.

Filter syntax:

::

    dictsort(value, case_sensitive=False, by='key')

After ``dictsort`` sorts the dictionary, it returns a list of tuples, not a dictionary.

Template example templates/filter_dictsort.txt using ``dictsort`` filter:

::

    {% for intf, params in trunks | dictsort %}
    interface {{ intf }}
     {% if params.action == 'add' %}
     switchport trunk allowed vlan add {{ params.vlans }}
     {% elif params.action == 'delete' %}
     switchport trunk allowed vlan remove {{ params.vlans }}
     {% else %}
     switchport trunk allowed vlan {{ params.vlans }}
     {% endif %}
    {% endfor %}

Note that filter awaits a dictionary, not a list of tuples or iterator.

Data file (data_files/filter_dictsort.yml):

.. code:: yaml

    trunks:
      Fa0/2:
        action: only
        vlans: 10,30
      Fa0/3:
        action: delete
        vlans: 10
      Fa0/1:
        action: add
        vlans: 10,20

The result of execution will be (interfaces are ordered):

::

    $ python cfg_gen.py templates/filter_dictsort.txt data_files/filter_dictsort.yml
    interface Fa0/1
     switchport trunk allowed vlan add 10,20
    interface Fa0/2
     switchport trunk allowed vlan 10,30
    interface Fa0/3
     switchport trunk allowed vlan remove 10

join
~~~~

Filter ``join`` works just like join() method in Python.

With ``join`` filter you can combine sequence of elements into a string with
an optional separator between elements.

Template example templates/filter_join.txt using ``join`` filter:

::

    {% for intf, params in trunks | dictsort %}
    interface {{ intf }}
     {% if params.action == 'add' %}
     switchport trunk allowed vlan add {{ params.vlans | join(',') }}
     {% elif params.action == 'delete' %}
     switchport trunk allowed vlan remove {{ params.vlans | join(',') }}
     {% else %}
     switchport trunk allowed vlan {{ params.vlans | join(',') }}
     {% endif %}
    {% endfor %}

Data file (data_files/filter_join.yml):

.. code:: yaml

    trunks:
      Fa0/1:
        action: add
        vlans:
          - 10
          - 20
      Fa0/2:
        action: only
        vlans:
          - 10
          - 30
      Fa0/3:
        action: delete
        vlans:
          - 10

The result of execution:

::

    $ python cfg_gen.py templates/filter_join.txt data_files/filter_join.yml
    interface Fa0/1
     switchport trunk allowed vlan add 10,20
    interface Fa0/2
     switchport trunk allowed vlan 10,30
    interface Fa0/3
     switchport trunk allowed vlan remove 10

