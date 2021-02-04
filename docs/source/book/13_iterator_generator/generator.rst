Generator
---------------------

Generators are a special class of functions that allows you to easily create
your own iterators. Unlike regular functions, a generator doesn't just
return a value and exit, but returns an iterator that returns the elements one at a time.

.. note::

    Generators are not covered in this book and are only mentioned here because
    they are a fairly straightforward way to create iterators.
    `More about generators <https://advpyneng.readthedocs.io/ru/latest/book/14_generators/further_reading.html>`__

A normal function exits if:

* execution reached the ``return`` statement
* function code is ended (this works as ``return None`` expression) 
* an exception raised

After function execution is finished the control is returned and program
execution goes further. All arguments that were passed to function like
local variables, all of this is lost. Only the result that returned
a function remains.

A function can return a list of elements, multiple objects or different results
depending on arguments, but it always returns a single result.

Generator generates values. Values are then returned on demand and after return
of one value a function-generator is suspended until the next value is
requested. Between requests, generator retains its state.

Python allows generators to be created in two ways:

* generator expression
* generator function


generator expression
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Generator expression uses the same syntax as a list comprehensions, but returns
iterator, not list (notе the parentheses instead of the square brackets):

.. code:: python

    In [1]: genexpr = (x**2 for x in range(10000))

    In [2]: genexpr
    Out[2]: <generator object <genexpr> at 0xb571ec8c>

    In [3]: next(genexpr)
    Out[3]: 0

    In [4]: next(genexpr)
    Out[4]: 1

    In [5]: next(genexpr)
    Out[5]: 4


It is useful when working with a large iterable object or infinite iterator.

