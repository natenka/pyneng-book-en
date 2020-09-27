for/else, while/else
--------------------

In the loops **for** and **while** you may optionally use **else** block.

for/else
~~~~~~~~

In the loop **for**:

* block **else** is executed if the loop has completed the iteration of the list
* but it *does not execute* if **break** was applied in the loop.

Example of a loop **for** with **else** (block **else** is executed after loop **for**):

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

An example of a loop **for** with **else** and **break** in the loop (because of **break** the block **else** is not applied):

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

Example of the loop **for** with **else** and **continue** in the loop (**continue** does not affect the **else** block):

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

In the loop **while**:

* block **else** is executed if the loop has completed the iteration of the list
* but it *does not execute* if **break** was applied in the loop.

Example of a loop **while** with **else** (the block **else** runs after the loop **while**):

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

An example of a loop **while** with **else** and **break** in a loop (because of **break** the block **else** is not applied):

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

