.. raw:: latex

   \newpage

.. _files_index:

7. Working with files
============================

In real life, in order to make full use of everything covered before this section you need to understand how to work with files.

When working with network equipment (and not only), files can be:

* configurations (simple, non-structured text files)

  * They are discussed in this section

* configuration templates
  
  * usually a special file format.
  * section `Jinja configuration temlates <../21_jinja2/>`__ discusses the use of Jinja2 to create configuration templates
    
* files with connection options

  * usually they are structured files in some particular format: YAML, JSON, CSV

    * section `Data serialization <../17_serialization/>`__ discusses how to handle such files

* other Python scripts
  
  * section  `Modules <../11_modules/>`__ discusses how to work with modules (other Python scripts)

This section covers simple text files. For example, Cisco configuration file.

There are several aspects to working with files:

* opening/closing
* reading
* writing

This section covers only the minimum required for working with files. More in 
`Python documentation <https://docs.python.org/3/library/stdtypes.html#bltin-file-objects>`__.

.. toctree::
   :maxdepth: 1

   open
   read
   write
   close
   with
   further_reading
   ../../exercises/07_exercises
