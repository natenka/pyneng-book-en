.. raw:: latex

   \newpage

.. _serialization_index:

17. Working with CSV, JSON, YAML files
============================

Data serialization is about storing data in some format that is often structured.

For example, it could be:

* files in YAML or JSON format
* files in CSV format
* database

In addition, Python allows you to write down objects of language itself (this
aspect is not covered, but if you are interested, look at the Pickle module).

This section covers CSV, JSON, YAML formats and chapter 25 covers databases.

YAML, JSON, CSV formats usage:

* you may have data about IP address and similar information to process in tables

  * table can be exported to CSV format and processed by Python 

* software can return data in JSON format. Accordingly, by converting this data into a Python object you can work with it and do whatever you want 
* YAML is very convenient to use to describe parameters because it has a nice syntax

  * for example, it can be settings for different objects (IP addresses, VLANs, etc.)
  * at least knowing YAML format will be useful when using Ansible

For each of these formats, Python has a module that makes them easier to work with.

.. toctree::
   :maxdepth: 1

   csv
   json
   yaml
   further_reading
   ../../exercises/17_exercises
