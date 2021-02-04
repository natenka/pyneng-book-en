for
---

Very often the same step should be performed for a set of the same data type. For example, convert all strings in list to uppercase. Python uses ``for`` loop for such purposes.

``For`` loop iterates elements of specified sequence and performs actions
specified for each element.

Examples of sequences of elements that can be iterated by ``for``:

-  string
-  list
-  dictionary
-  :ref:`range`
-  Any :ref:`iterable`

An example of converting strings in a list to uppercase without ``for`` loop:

.. code:: python

    In [1]: words = ['list', 'dict', 'tuple']

    In [2]: upper_words = []

    In [3]: words[0]
    Out[3]: 'list'

    In [4]: words[0].upper() # converting word to uppercase
    Out[4]: 'LIST'

    In [5]: upper_words.append(words[0].upper()) # converting and adding to new list

    In [6]: upper_words
    Out[6]: ['LIST']

    In [7]: upper_words.append(words[1].upper())

    In [8]: upper_words.append(words[2].upper())

    In [9]: upper_words
    Out[9]: ['LIST', 'DICT', 'TUPLE']


This solution has several nuances:

* the same action need to be repeated several times
* code is tied to a certain number of elements in *words* list


The same steps with the ``for`` loop:
 
.. code:: python

    In [10]: words = ['list', 'dict', 'tuple']

    In [11]: upper_words = []

    In [12]: for word in words:
        ...:     upper_words.append(word.upper())
        ...:

    In [13]: upper_words
    Out[13]: ['LIST', 'DICT', 'TUPLE']

Expression ``for word in words: upper_words.append(word.upper())``
means "for each word in ``words`` list to perform actions in block ``for``".
In this case, ``word`` is the name of the variable, which refers to different
values each iteration of the loop.

.. note::
    The `pythontutor <http://www.pythontutor.com/>`__ project can be very helpful in understanding loops.
    The project visualize code execution and allows you to see what happens at every stage
    of code execution, which is especially useful in first steps of learning loops.
    The `pythontutor <http://www.pythontutor.com/visualize.html#mode=edit>`__ allows you to upload your code, for instance, see `example above <http://www.pythontutor.com/visualize.html#code=words%20%3D%20%5B'list',%20'dict',%20'tuple'%5D%0Aupper_words%20%3D%20%5B%5D%0A%0Afor%20word%20in%20words%3A%0A%20%20%20%20upper_words.append%28word.upper%28%29%29%0A&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false>`__.


``For`` loop can work with any sequence of elements.
For example, the above code used a list and the loop iterated over the elements of the list.
The for loop works in a similar way with tuples.


When working with strings ``for`` loop iterates through string characters, for example:

.. code:: python

    In [1]: for letter in 'Test string':
       ...:     print(letter)
       ...:
    T
    e
    s
    t

    s
    t
    r
    i
    n
    g

.. note::

    Loop uses a variable named *letter*. Although, it could be any name, it is better when name tells you which objects go through a loop.

Sometimes it is necessary to use sequence of numbers in loop. In this case, it is best to use
:ref:`range`

Example of loop ``for`` with range() function:

.. code:: python

    In [2]: for i in range(10):
       ...:     print('interface FastEthernet0/{}'.format(i))
       ...:
    interface FastEthernet0/0
    interface FastEthernet0/1
    interface FastEthernet0/2
    interface FastEthernet0/3
    interface FastEthernet0/4
    interface FastEthernet0/5
    interface FastEthernet0/6
    interface FastEthernet0/7
    interface FastEthernet0/8
    interface FastEthernet0/9

This loop uses ``range(10)``. Function range() generates numbers in range from zero to specified number (in this example, up to 10) not including it.

In this example, loop runs through *vlans* list, so variable can be called *vlan*:

.. code:: python

    In [3]: vlans = [10, 20, 30, 40, 100]
    In [4]: for vlan in vlans:
       ...:     print('vlan {}'.format(vlan))
       ...:     print(' name VLAN_{}'.format(vlan))
       ...:
    vlan 10
     name VLAN_10
    vlan 20
     name VLAN_20
    vlan 30
     name VLAN_30
    vlan 40
     name VLAN_40
    vlan 100
     name VLAN_100


When a loop runs through dictionary, it actually goes through keys:

.. code:: python

    In [34]: r1 = {
        ...:      'ios': '15.4',
        ...:      'ip': '10.255.0.1',
        ...:      'hostname': 'london_r1',
        ...:      'location': '21 New Globe Walk',
        ...:      'model': '4451',
        ...:      'vendor': 'Cisco'}
        ...:

    In [35]: for k in r1:
        ...:     print(k)
        ...:
    ios
    ip
    hostname
    location
    model
    vendor


If you want to print key-value pairs in loop, you can do this:

.. code:: python

    In [36]: for key in r1:
        ...:     print(key + ' => ' + r1[key])
        ...:
    ios => 15.4
    ip => 10.255.0.1
    hostname => london_r1
    location => 21 New Globe Walk
    model => 4451
    vendor => Cisco

Or use items() method which allows you to run loop over a key-value pair:

.. code:: python

    In [37]: for key, value in r1.items():
        ...:     print(key + ' => ' + value)
        ...:
    ios => 15.4
    ip => 10.255.0.1
    hostname => london_r1
    location => 21 New Globe Walk
    model => 4451
    vendor => Cisco


Method items() returns a special view object that displays key-value pairs:

.. code:: python

    In [38]: r1.items()
    Out[38]: dict_items([('ios', '15.4'), ('ip', '10.255.0.1'), ('hostname', 'london_r1'), ('location', '21 New Globe Walk'), ('model', '4451'), ('vendor', 'Cisco')])


.. toctree::
   :maxdepth: 1

   for_in_for
   for_if
