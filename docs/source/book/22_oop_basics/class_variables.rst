Class variables
~~~~~~~~~~~~~~~~~

In addition to instance variables, there are also class variables. They are
created when variables are specified within class itself, not method:

.. code:: python

    In [27]: class A:
        ...:     var_a = 5
        ...:
        ...:     def method(self):
        ...:         pass
        ...:

Now not only class but every instance of the class will have ``var_a`` variable:

.. code:: python

    In [40]: A.var_a
    Out[40]: 5

    In [30]: a1 = A()

    In [31]: a1.var_a
    Out[31]: 5

    In [32]: a2 = A()

    In [33]: a2.var_a
    Out[33]: 5

An important point when using class variables is that within method they should
still be called  through name of the class (or ``self``, but through name of the
class better because then it is clear that it is a class variable). First,
version without class name:

.. code:: python

    In [37]: class A:
        ...:     var_a = 5
        ...:
        ...:     def method(self):
        ...:         print(var_a)
        ...:

    In [38]: a1 = A()

    In [39]: a1.method()
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    <ipython-input-39-921b8753dbee> in <module>()
    ----> 1 a1.method()

    <ipython-input-37-ef925c4e39d3> in method(self)
          3
          4     def method(self):
    ----> 5         print(var_a)
          6

    NameError: name 'var_a' is not defined

And correct version:

.. code:: python

    In [47]: class A:
        ...:     var_a = 5
        ...:
        ...:     def method(self):
        ...:         print(A.var_a)
        ...:

    In [48]: a1 = A()

    In [49]: a1.method()
    5

