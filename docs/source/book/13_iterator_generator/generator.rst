Generator
---------------------

Generators are a special class of functions that can easily create their own iterators. Unlike normal functions, the generator does not just return the value and finish the work, but returns the iterator which gives the elements one by one.

The usual function ends if:

* ``return`` expression is met
* function code is ended (this works as ``return None`` expression) 
* exception has arisen

After function execution is finished, the control is returned and program execution goes further. All the arguments that were passed to the function, the local variables, all of this is lost. Only the result that returned the function remains.

A function can return a list of elements, multiple objects or different results depending on the arguments, but it always returns a single result.

The generator generates values. The values are then returned on demand and after the return of one value the function-generator is suspended until the next value is requested. Between requests, the generator retains its state.

Python allows generators to be created in two ways:

* generator expression
* generator function

The following is an example of a generator expression and a
`separate note <https://natenka.github.io/python/fluent-python-generator/>`__ for generator functions

generator expression
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The generator expression uses the same syntax as the list comprehensions, but returns the iterator, not the list.

The generator expression looks exactly the same as the list comprehensions, but the brackets are used:

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

Note that this is not a tuple comprehensions but a generator expression.

It is useful when working with a large iterable object or infinite iterator.
