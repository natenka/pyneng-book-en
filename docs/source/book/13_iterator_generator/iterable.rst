.. _iterable:

Iterable object
------------------

Iteration is a generic term that describes the procedure for taking elements of something in turn.

In a more general sense, it is a sequence of instructions that is repeated a certain number of times or before the specified condition is fulfilled.

An iterable object is an object that can return elements one at a time. It is also an object from which an iterator can be derived.

Examples of iterable objects:

* all sequences: list, string, tuple
* dictionaries 
* files

In Python the iter() function is responsible for iterator deriving.

.. code:: python

    In [1]: lista = [1, 2, 3]

    In [2]: iter(lista)
    Out[2]: <list_iterator at 0xb4ede28c>

``iter()`` function will work on any object that has ``__iter__`` or  ``__getitem__`` method.

``__iter__`` method returns an iterator. If this method is not available, iter() function checks if there is ``__getitem__`` method that allows getting elements by index.

If method ``__getitem__`` is present an iterator is returned, which iterates through the elements using index (starting with 0).

In practice, the use of ``__getitem__`` means that all sequence elements are iterable objects. For example, a list, a tuple, a string. Although these data types have ``__iter__`` method.
