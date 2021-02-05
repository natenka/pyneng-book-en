II. Code reuse
################################

When writing code, some of the steps are often repeated. It can be a small
block of 3-5 lines, or it can be a fairly large sequence of steps.
Copying code is a bad idea. Because if you have to update one of copies later,
you have to update others.

Instead, you create a special code block with name - function. And every
time code has to be repeated, you just call a function. Function allows
not only to name a block of code but also to make it more abstract through
parameters. Parameters make it possible to pass different data
for function. And get different results depending on input parameters.

Section :ref:`functions_index` covers with creation of functions,
section :ref:`useful_functions_index` discusses useful built-in functions.

Once code is divided into functions, there comes a time when you
need to use function in another script. Of course, copying a
function is as inconvenient as copying a normal code. Modules are used
to reuse code from another Python script.

Section :ref:`modules_index` is dedicated to creating your own modules
and section :ref:`useful_modules_index` covers useful modules
from Python standard library. 

The last section :ref:`iterator_generator_index` is dedicated to iterable
objects, iterators and generators.

.. toctree::
   :maxdepth: 1

   09_functions/index.rst
   10_useful_functions/index.rst
   11_modules/index.rst
   12_useful_modules/index.rst
   13_iterator_generator/index.rst
