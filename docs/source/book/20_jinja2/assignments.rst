set
---

You can assign values to variables inside template. These can be new variables
or there may be modified values of variables that have been passed to template.
In this way you can remember a value that for example was obtained by using
several filters. Then use variable name instead of repeating all filters.

Template example templates/set.txt in which **set** expression is used to
specify shorter parameter names:

::

    {% for intf, params in trunks | dictsort %}
     {% set vlans = params.vlans %}
     {% set action = params.action %}

    interface {{ intf }}
     {% if vlans is iterable %}
      {% if action == 'add' %}
     switchport trunk allowed vlan add {{ vlans | join(',') }}
      {% elif action == 'delete' %}
     switchport trunk allowed vlan remove {{ vlans | join(',') }}
      {% else %}
     switchport trunk allowed vlan {{ vlans | join(',') }}
      {% endif %}
     {% else %}
      {% if action == 'add' %}
     switchport trunk allowed vlan add {{ vlans }}
      {% elif action == 'delete' %}
     switchport trunk allowed vlan remove {{ vlans }}
      {% else %}
     switchport trunk allowed vlan {{ vlans }}
      {% endif %}
     {% endif %}
    {% endfor %}

Note the second and third lines:

::

     {% set vlans = params.vlans %}
     {% set action = params.action %}

In this way new variables are created and these new values are used. It makes
template look clearer.

Data file (data_files/set.yml):

.. code:: json

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

The result of execution:

::

    $ python cfg_gen.py templates/set.txt data_files/set.yml

    interface Fa0/1
     switchport trunk allowed vlan add 10,20

    interface Fa0/2
     switchport trunk allowed vlan 10,30

    interface Fa0/3
     switchport trunk allowed vlan remove 10

