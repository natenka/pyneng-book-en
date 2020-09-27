.. raw:: latex

   \newpage

.. _functions_index:

9. Functions
============================

A function is a block of code that performs certain actions:

* function has a name to run this code block as many times as you want

  * launch of function code is called a function call

* function parameters are usually defined when creating a function.

  * function parameters determine which arguments a function can accept
  * arguments can be passed to functions
  * hence, the function code will be executed according to the stated arguments

**What are the functions for?**

Typically, the problems that code solves are very similar and often have something in common.

For example, when working with configuration files each time it is necessary to perform such actions:

* file opening
* deletion (or skipping) of lines starting with the exclamation mark (for Cisco)
* deleting (or skipping) empty lines
* deleting line feed characters at the end of lines
* converting the result to a list

Beyond that, actions can vary depending on what needs to be done.

Often there’s a piece of code that repeats itself. Of course, you can copy it from one script to another. But this is very inconvenient because when you change the code you have to update it in all the files in which it is copied.

It is much easier and more accurate to put this code into a function (it can also be several functions).

And then you will call this function - in this file or another one.

This section discusses when a function is in the same file.

And in :ref:`modules_index` we will see how to reuse objects that are in other scripts.

.. toctree::
   :maxdepth: 1

   1_create_func
   2_namespace
   3_func_params_args
   4_func_unpacking_and_var_args_example
   further_reading
   ../../exercises/09_exercises
