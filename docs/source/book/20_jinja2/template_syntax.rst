Jinja2 template syntax
-------------------------

So far, only variable substitution has been used in Jinja2 template examples. This is the simplest and most understandable example of using templates. But syntax of Jinja templates is not limited to this.

In Jinja2 templates you can use :

* variables 
* conditions  (if/else)
* loops (for) 
* filters - special built-in methods that allow to convert variables
* tests - are used to check whether a variable matches a condition

In addition, Jinja supports inheritance between templates and also allows adding the contents of one template to another.
This section covers only few possibilities. More information about Jinja2 templates can be found in `documentation <http://jinja.pocoo.org/docs/dev/templates/>`__.

.. note::

    All files used as examples in this subsection are in  3_template_syntax/ directory

Script cfg_gen.py will be used to generate templates.

.. code:: python

    # -*- coding: utf-8 -*-
    from jinja2 import Environment, FileSystemLoader
    import yaml
    import sys
    import os

    #$ python cfg_gen.py templates/for.txt data_files/for.yml
    template_dir, template = os.path.split(sys.argv[1])

    vars_file = sys.argv[2]

    env = Environment(
        loader=FileSystemLoader(template_dir),
        trim_blocks=True,
        lstrip_blocks=True)
    template = env.get_template(template_file)

    with open(vars_file) as f:
        vars_dict = yaml.safe_load(f)

    print(template.render(vars_dict))

In order to see the result, you have to call the script and give it two arguments:

* template 
* file with variables in YAML format

The result will be displayed on standard output stream.

Example of script run:

::

    $ python cfg_gen.py templates/variables.txt data_files/vars.yml

Parameters trim_blocks and lstrip_blocks are described in the following subsection.

.. toctree::
   :maxdepth: 1

   whitespace_control
   syntax_variables
   syntax_for
   syntax_if
   syntax_filter
   syntax_test
   assignments
   include
