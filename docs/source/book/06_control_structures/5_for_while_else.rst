for/else, while/else
--------------------

In loops **for** and **while** you may optionally use **else** block.

for/else
~~~~~~~~

In loop **for**:

* block **else** is executed if loop has completed iteration of list
* but it *does not execute* if **break** was applied in loop.

Example of loop **for** with **else** (block **else** is executed after loop **for**):

.. code:: python

    In [1]: for num in range(5):
       ....:     print(num)
       ....: else:
       ....:     print("Run out of numbers")
       ....:     
    0
    1
    2
    3
    4
    Run out of numbers

An example of loop **for** with **else** and **break** in loop (because of **break**,  block **else** is not applied):

.. code:: python

    In [2]: for num in range(5):
       ....:     if num == 3:
       ....:         break
       ....:     else:
       ....:         print(num)
       ....: else:
       ....:     print("Run out of numbers")
       ....:     
    0
    1
    2

Example of loop **for** with **else** and **continue** in loop (**continue** does not affect **else** block):

.. code:: python

    In [3]: for num in range(5):
       ....:     if num == 3:
       ....:         continue
       ....:     else:
       ....:         print(num)
       ....: else:
       ....:     print("Run out of numbers")
       ....:     
    0
    1
    2
    4
    Run out of numbers

while/else
~~~~~~~~~~

In loop **while**:

* block **else** is executed if loop has completed iteration of list
* but it *does not execute* if **break** was applied in loop.

Example of a loop **while** with **else** (block **else** runs after loop **while**):

.. code:: python

    In [4]: i = 0
    In [5]: while i < 5:
       ....:     print(i)
       ....:     i += 1
       ....: else:
       ....:     print("Конец")
       ....:     
    0
    1
    2
    3
    4
    Конец

An example of a loop **while** with **else** and **break** in loop (because of **break**, block **else** is not applied):

.. code:: python

    In [6]: i = 0

    In [7]: while i < 5:
       ....:     if i == 3:
       ....:         break
       ....:     else:
       ....:         print(i)
       ....:         i += 1
       ....: else:
       ....:     print("Конец")
       ....:     
    0
    1
    2

