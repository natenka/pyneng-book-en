.. raw:: latex

   \newpage

.. _module_re_index:

15. Module re
============================

Python uses ``re`` module to work with regular expressions.

Core functions of ``re`` module: 

* ``match`` - searches a sequence at the beginning of the line
* ``search`` - searches for first match with template
* ``findall`` - searches for all matches with template. Returns the resulting strings as a list 
* ``finditer`` - searches for any matches with template. Returns an iterator
* ``compile`` - compiles regex. You can then apply all of listed functions to this object
* ``fullmatch`` - the entire line must conform to regex described

In addition to functions that search matches, module has the following functions:

-  ``re.sub`` - for replacement in strings
-  ``re.split`` - to split string into parts


.. toctree::
   :maxdepth: 1
   :hidden:

   match_object
   search
   match
   finditer
   findall
   compile
   flags
   split
   sub
   further_reading
   ../../exercises/15_exercises
