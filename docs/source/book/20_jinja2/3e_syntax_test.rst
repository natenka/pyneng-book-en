Tests
-----

Besides filters, Jinja also supports tests. Tests allow variables to be tested for a certain condition.

Jinja supports a large number of built-in tests. We will look at only a few of them. The rest of tests you can find in `documentation <http://jinja.pocoo.org/docs/dev/templates/#builtin-tests>`__.

Tests, like filters, can be created by yourself.

defined
~~~~~~~

Test **defined** allows you to check if variable is present in the data dictionary.

Template example templates/test_defined.txt:

::

    router ospf 1
    {% if ref_bw is defined %}
     auto-cost reference-bandwidth {{ ref_bw }}
    {% else %}
     auto-cost reference-bandwidth 10000
    {% endif %}
    {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
    {% endfor %}

This example is more cumbersome than **default** filter option, but this test may be useful if depending on whether a variable is defined or not, different commands need to be executed.

Data file (data_files/test_defined.yml):

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

    $ python cfg_gen.py templates/test_defined.txt data_files/test_defined.yml
    router ospf 1
     auto-cost reference-bandwidth 10000
     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0

iterable
~~~~~~~~

Test **iterable** checks whether the object is an iterator.

Due to these checks, it is possible to make branches in template which will take into account the type of variable.

Template templates/test_iterable.txt (indents made to make an idea of branches more clear):

::

    {% for intf, params in trunks | dictsort %}
    interface {{ intf }}
     {% if params.vlans is iterable %}
       {% if params.action == 'add' %}
     switchport trunk allowed vlan add {{ params.vlans | join(',') }}
       {% elif params.action == 'delete' %}
     switchport trunk allowed vlan remove {{ params.vlans | join(',') }}
       {% else %}
     switchport trunk allowed vlan {{ params.vlans | join(',') }}
       {% endif %}
     {% else %}
       {% if params.action == 'add' %}
     switchport trunk allowed vlan add {{ params.vlans }}
       {% elif params.action == 'delete' %}
     switchport trunk allowed vlan remove {{ params.vlans }}
       {% else %}
     switchport trunk allowed vlan {{ params.vlans }}
       {% endif %}
     {% endif %}
    {% endfor %}

Data file (data_files/test_iterable.yml):

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
        vlans: 10

Note the last line: ``vlans: 10``. In this case, 10 is no longer in the list and **join** filter does not work. But, due to ``is iterable`` test (in this case the result will be false), in this case template goes into *else* branch.

The result of execution:

::

    $ python cfg_gen.py templates/test_iterable.txt data_files/test_iterable.yml
    interface Fa0/1
     switchport trunk allowed vlan add 10,20
    interface Fa0/2
     switchport trunk allowed vlan 10,30
    interface Fa0/3
     switchport trunk allowed vlan remove 10


Such indents appeared because the template uses indents but does not have *lstrip_blocks=True* installed (it removes spaces and tabs at the beginning of the line).

