for
---

Very often the same action should be performed for a set of the same data type. For example, convert all strings in the list to uppercase. Python uses ``for`` loop for such purposes.

Loop **for** iterates elements of specified sequence and performs the actions specified for each element.

Examples of sequences of elements that can be iterated by **for**:

-  string
-  list
-  dictionary
-  :ref:`range`
-  Any :ref:`iterable`

An example of converting strings in a list to uppercase without a loop **for**:

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
* code is tied to a certain number of elements in **words** list


Same actions with loop **for**:

.. code:: python

    In [10]: words = ['list', 'dict', 'tuple']

    In [11]: upper_words = []

    In [12]: for word in words:
        ...:     upper_words.append(word.upper())
        ...:

    In [13]: upper_words
    Out[13]: ['LIST', 'DICT', 'TUPLE']

The expression ``for word in words: upper_words.append(word.upper())``
means "for each word in the **words** list to perform actions in the block **for**".
Note, that **word** is the name of variable that refers to different values for each iteration of the loop.

.. note::
    The ` pythontutor <http://www.pythontutor.com/>`__ project can help to understand the loops. There is a special visualization of the code that allows you to see what happens at every stage of the code execution, which is especially useful in the first steps of learning loops. The `pythontutor <http://www.pythontutor.com/visualize.html#mode=edit>`__ allows you to upload your code but, for instance, you can see `example above <http://www.pythontutor.com/visualize.html#code=words%20%3D%20%5B'list',%20'dict',%20'tuple'%5D%0Aupper_words%20%3D%20%5B%5D%0A%0Afor%20word%20in%20words%3A%0A%20%20%20%20upper_words.append%28word.upper%28%29%29%0A&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false>`__.


The **for** loop can work with any sequence of elements. For example, the list was used above and the loop iterates through the list elements. Similarly, **for** works with tuples.

When working with strings **for** loop iterates through string characters, for example:

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
    The loop uses a variable named **letter**. Although the name can be any name, it is convenient when the name tells you which objects go through a loop.

Sometimes it is necessary to use sequence of numbers in the loop. In this case, it is best to use 
:ref:`range`

Example of a loop **for** with range() function:

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

This loop uses ``range(10)``. The range() function generates numbers in range from zero to the specified number (in this example, up to 10) not including it.

In this example, the loop runs through the Vlans list, so the variable can be called **vlan**:

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


When a loop runs through dictionary, it actually goes by the keys:

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


If you want to print key-value pairs in the loop, you can do this:

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

Or use the items() method which allows you to run the loop over a key-value pair:

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


The items() method returns a special view object that displays key-value pairs:

.. code:: python

    In [38]: r1.items()
    Out[38]: dict_items([('ios', '15.4'), ('ip', '10.255.0.1'), ('hostname', 'london_r1'), ('location', '21 New Globe Walk'), ('model', '4451'), ('vendor', 'Cisco')])


.. toctree::
   :maxdepth: 1

   2a_for_in_for
   2b_for_if
