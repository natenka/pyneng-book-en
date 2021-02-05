Template inheritance
---------------------

Template inheritance is a very powerful functionality that avoids repetition
of the same in different templates.

When using inheritance, there are:

* ``base template`` - template that describes template skeleton. 
    * this template may contain any ordinary expressions or text. In addition, special ``blocks`` are defined in this template.
* ``child template`` - template that extends base template by filling in specified blocks.
    * child templates can overwrite or supplement blocks defined in base template.

Example of base template templates/base_router.txt:

::

    !
    {% block services %}
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    {% endblock %}
    !
    no ip domain lookup
    !
    ip ssh version 2
    !
    {% block ospf %}
    router ospf 1
     auto-cost reference-bandwidth 10000
    {% endblock %}
    !
    {% block bgp %}
    {% endblock %}
    !
    {% block alias %}
    {% endblock %}
    !
    line con 0
     logging synchronous
     history size 100
    line vty 0 4
     logging synchronous
     history size 100
     transport input ssh
    !

Note four blocks that are created in template:

::

    {% block services %}
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    {% endblock %}
    !
    {% block ospf %}
    router ospf 1
     auto-cost reference-bandwidth 10000
    {% endblock %}
    !
    {% block bgp %}
    {% endblock %}
    !
    {% block alias %}
    {% endblock %}

These are blanks for the corresponding configuration sections. A child template
that uses this base template as a base can fill all or only some of the blocks.

Child template templates/hq_router.txt:

::

    {% extends "base_router.txt" %}

    {% block ospf %}
    {{ super() }}
    {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
    {% endfor %}
    {% endblock %}

    {% block alias %}
    alias configure sh do sh
    alias exec ospf sh run | s ^router ospf
    alias exec bri show ip int bri | exc unass
    alias exec id show int desc
    alias exec top sh proc cpu sorted | excl 0.00%__0.00%__0.00%
    alias exec c conf t
    alias exec diff sh archive config differences nvram:startup-config system:running-config
    alias exec desc sh int desc | ex down
    {% endblock %}

The first line in template templates/hq_router.txt is very important:

::

    {% extends "base_router.txt" %}

It is said that template hq_router.txt will be constructed on the basis of
template base_router.txt.

Inside child template, everything happens inside blocks. Due to the blocks
that have been defined in base template, child template can extend the parent template.

.. note::

    Note that lines described in child template outside blocks are ignored.

There are four blocks in base template: services, ospf, bgp, alias.
In child template only two of them are filled: ospf and alias.
That's the convenience of inheritance. You don't have to fill all blocks in every child template.

In this way *ospf* and *alias* blocks are used differently. In base template,
*ospf* block already has part of configuration:

::

    {% block ospf %}
    router ospf 1
     auto-cost reference-bandwidth 10000
    {% endblock %}

Therefore, child template has a choice: use this configuration and supplement
it or completely rewrite everything in child template.

In this case the configuration is supplemented. That is why in child template
templates/hq_router.txt the *ospf* block starts with expression 
``{{ super() }}``:

::

    {% block ospf %}
    {{ super() }}
     {% for networks in ospf %}
     network {{ networks.network }} area {{ networks.area }}
     {% endfor %}
    {% endblock %}

``{{ super() }}`` transfers content of this block from parent template to child
template. Because of this, lines from parent are moved to child template.

.. note::

    Expression ``super`` doesn't have to be at the beginning of the block. It
    could be anywhere in the block. Content of base template are moved to where
    ``super`` expression is located.

``alias`` block simply describes the alias. And even if there were some settings
in parent template, they would be substituted by content of child template.

Let's recap the rules for working with blocks. If block is created in parent template:

* no content - in child template you can fill this block or ignore it. If block
  is filled, it will contain only what was written in child template (example - ``alias`` block)
* with content - in child template you can perform such actions:

  * ignore block - in this case, child template will get content from parent template (example - *services* block)
  * rewrite block - then child template will contain only what it has 
  * move content of the block from parent template and supplement it - then
    child template will contain both the content of the block from parent
    template and the content from child template. To transfer content from
    parent template the expression ``{{ super() }}`` is used (example - *ospf* block)

Data file for template configuration generation 
(data_files/hq_router.yml):

.. code:: json

    ospf:
      - network: 10.0.1.0 0.0.0.255
        area: 0
      - network: 10.0.2.0 0.0.0.255
        area: 2
      - network: 10.1.1.0 0.0.0.255
        area: 0

The result will be:

::

    $ python cfg_gen.py templates/hq_router.txt data_files/hq_router.yml
    !
    service timestamps debug datetime msec localtime show-timezone year
    service timestamps log datetime msec localtime show-timezone year
    service password-encryption
    service sequence-numbers
    !
    no ip domain lookup
    !
    ip ssh version 2
    !
    router ospf 1
     auto-cost reference-bandwidth 10000

     network 10.0.1.0 0.0.0.255 area 0
     network 10.0.2.0 0.0.0.255 area 2
     network 10.1.1.0 0.0.0.255 area 0
    !
    !
    alias configure sh do sh
    alias exec ospf sh run | s ^router ospf
    alias exec bri show ip int bri | exc unass
    alias exec id show int desc
    alias exec top sh proc cpu sorted | excl 0.00%__0.00%__0.00%
    alias exec c conf t
    alias exec diff sh archive config differences nvram:startup-config system:running-config
    alias exec desc sh int desc | ex down
    !
    line con 0
     logging synchronous
     history size 100
    line vty 0 4
     logging synchronous
     history size 100
     transport input ssh
    !

Note that in *ospf* block there are commands from base template and commands from child template.

