Control of whitespace symbols
----------------------------

trim_blocks, lstrip_blocks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Parameter ``trim_blocks`` removes the first empty line after block if its value
is True (default False).

Effect of using the flag is showed on example templates/env_flags.txt:

::

    router bgp {{ bgp.local_as }}
     {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

If cfg_gen.py script starts without trim_blocks,
lstrip_blocks:

.. code:: python

    env = Environment(loader=FileSystemLoader(TEMPLATE_DIR))

The output  is:

::

    $ python cfg_gen.py templates/env_flags.txt data_files/router.yml
    router bgp 100

     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100

     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

new lines occur because of *for* block.

::

    {% for ibgp in bgp.ibgp_neighbors %}

By default, the same behavior will be with any other Jinja blocks.

When trim_blocks flag is added:

.. code:: python

    env = Environment(loader=FileSystemLoader(TEMPLATE_DIR),
                      trim_blocks=True)

The result will be:

::

    $ python cfg_gen.py templates/env_flags.txt data_files/router.yml
    router bgp 100
      neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
      neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

Empty lines after block were removed.

In front of ``neighbor ... remote-as`` lines two spaces appeared. This is
because there is a space in front of *for* block. Once lstrip_blocks has
been disabled, spaces and tabs in front of the block are added to the first line of block.

This does not affect the next lines. Therefore, lines with 
``neighbor ... update-source`` are displayed with one space.

Parameter ``lstrip_blocks`` controls whether spaces and tabs will be 
removed from the beginning of line to the beginning of block (untill opening curly bracket).

If add ``lstrip_blocks=True``:

.. code:: python

    env = Environment(loader=FileSystemLoader(TEMPLATE_DIR),
                      trim_blocks=True, lstrip_blocks=True)

The result will be:

::

    $ python cfg_gen.py templates/env_flags.txt data_files/router.yml
    router bgp 100
     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

Disabling lstrip_blocks for block
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sometimes you need to disable lstrip_blocks in block.

For example, if ``lstrip_blocks`` is set to True in an environment, but must
be disabled for the second block in template (templates/flagenv_s2.txt file):

::

    router bgp {{ bgp.local_as }}
     {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

    router bgp {{ bgp.local_as }}
     {%+ for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

The result will be:

::

    $ python cfg_gen.py templates/env_flags2.txt data_files/router.yml
    router bgp 100
     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

    router bgp 100
      neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

Plus sign after percent sign disables lstrip_blocks for the block, in this case, only in the beginning.

If done this way (plus is added in the end block expression):

::

    router bgp {{ bgp.local_as }}
     {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

    router bgp {{ bgp.local_as }}
     {%+ for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {%+ endfor %}

It will be disabled for the end of the block:

::

    $ python cfg_gen.py templates/env_flags2.txt data_files/router.yml
    router bgp 100
     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

    router bgp 100
      neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
      neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

Removing whitespace from block
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similarly, you can control whitespace removal for a block.

For this example, flags are not set in environment:

::

    env = Environment(loader=FileSystemLoader(TEMPLATE_DIR))

Template templates/env_flags3.txt:

::

    router bgp {{ bgp.local_as }}
     {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

    router bgp {{ bgp.local_as }}
     {%- for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

Note the minus at the beginning of second block. Minus removes all whitespace
characters, in this case, at the beginning of the block.

The result will be:

::

    $ python cfg_gen.py templates/env_flags3.txt data_files/router.yml
    router bgp 100

     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100

     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100


    router bgp 100
     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100

     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

If you add minus to the end of the block:

::

    router bgp {{ bgp.local_as }}
     {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

    router bgp {{ bgp.local_as }}
     {%- for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {%- endfor %}

Empty string at the end of the block will be deleted:

::

    $ python cfg_gen.py templates/env_flags3.txt data_files/router.yml
    router bgp 100

     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100

     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100


    router bgp 100
     neighbor 10.0.0.2 remote-as 100
     neighbor 10.0.0.2 update-source lo100
     neighbor 10.0.0.3 remote-as 100
     neighbor 10.0.0.3 update-source lo100

Try to add minus at the end of expressions describing the block and look at the result:

::

    router bgp {{ bgp.local_as }}
     {% for ibgp in bgp.ibgp_neighbors %}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {% endfor %}

    router bgp {{ bgp.local_as }}
     {%- for ibgp in bgp.ibgp_neighbors -%}
     neighbor {{ ibgp }} remote-as {{ bgp.local_as }}
     neighbor {{ ibgp }} update-source {{ bgp.loopback }}
     {%- endfor -%}

