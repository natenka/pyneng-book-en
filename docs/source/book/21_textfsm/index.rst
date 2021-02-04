.. raw:: latex

   \newpage

.. _textfsm_index:

21. Parsing command output with TextFSM
===================================

On network devices that does not support any software interface, the output of
show commands is returned as a string. And although it's partly structured,
it's still just a string. And it has to be processed in some way to get
Python objects, like a dictionary or a list.

For example, it is possible to handle the output of a command line by line using
regular expressions to get Python objects. But there is a more convenient option: TextFSM

TextFSM - a library created by Google to handle output from network devices. It
allows you to create templates to process the output of a command.

Using TextFSM is better than simple line processing, as templates give a better
idea of how output will be handled and templates are easier to share. That means
it's easier to find templates that have already been created and use them or share your own.

.. toctree::
   :maxdepth: 1

   README
   textfsm_syntax
   textfsm_examples
   textfsm_clitable
   further_reading
   ../../exercises/21_exercises
