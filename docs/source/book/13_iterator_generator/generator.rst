Generator
---------------------

Generators are a special class of functions that can easily create their own iterators. Unlike normal functions, generator does not just return value and finish work, but returns an iterator which gives elements one by one.

Usual function ends if:

* ``return`` expression is met
* function code is ended (this works as ``return None`` expression) 
* exception has arisen

After function execution is finished the control is returned and program execution goes further. All arguments that were passed to function like local variables, all of this is lost. Only the result that returned a function remains.

A function can return a list of elements, multiple objects or different results depending on arguments, but it always returns a single result.

Generator generates values. Values are then returned on demand and after return of one value a function-generator is suspended until the next value is requested. Between requests, generator retains its state.

Python allows generators to be created in two ways:

* generator expression
* generator function

The following is an example of a generator expression and a
`separate note <https://natenka.github.io/python/fluent-python-generator/>`__ for generator functions

generator expression
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Generator expression uses the same syntax as a list comprehensions, but returns iterator, not list.

Generator expression looks exactly the same as a list comprehensions, but brackets are used:

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
