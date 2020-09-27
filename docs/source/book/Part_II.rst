II. Code reuse
################################

When the code is written you will find that some of the actions are often repeated. It can be a small block of 3-5 lines or it can be a rather large sequence of actions.

Copying code is a bad idea. Because if you have to update one of the copies later, you have to update the others.

Instead, you create a special code block with the name - function. And every time the code has to be repeated, you just call a function. The function allows not only to name a block of code but also to make it more abstract through parameters. The parameters make it possible to transfer different source data for the execution of the function. And, correspondingly, get different results depending on the input parameters.

Section :ref:`functions_index` deals with the creation of functions.
In addition, section :ref:`useful_functions_index` considers useful embedded functions.

Once the code is divided into functions, there comes a time when you need to use the function in another script. Of course, copying a function is as inconvenient as copying a normal code. Modules are used to reuse code from another Python script.

Section :ref:`modules_index` is dedicated to creating your own modules and section :ref:`useful_modules_index` is considered useful modules from the standard Python library. 

The last section :ref:`iterator_generator_index` is dedicated to iterable objects, iterators and generators.

.. toctree::
   :maxdepth: 1

   09_functions/index.rst
   10_useful_functions/index.rst
   11_modules/index.rst
   12_useful_modules/index.rst
   13_iterator_generator/index.rst
