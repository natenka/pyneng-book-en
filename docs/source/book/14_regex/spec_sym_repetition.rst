Repeating characters
------------------

*  ``regex+`` - one or more repetitions of preceding element
*  ``regex*`` - zero or more repetitions of preceding element
*  ``regex?`` -- zero or one repetition of preceding element
*  ``regex{n}`` - exactly 'n' repetitions of preceding element
*  ``regex{n,m}`` - from ‘n’ to ‘m’ repetitions of preceding element
*  ``regex{n,}`` - ‘n’ or more repetitions of preceding element

``+``
~~~~~

Plus indicates that the previous expression can be repeated as many times as you like, but at least once.

For example, here the repetition refers to letter 'a':

.. code:: py

    In [1]: line = '100     aab1.a1a1.a5d3    FastEthernet0/1'

    In [2]: re.search('a+', line).group()
    Out[2]: 'aa'

And in this expression, string ‘a1’ is repeated:

.. code:: py

    In [3]: line = '100     aab1.a1a1.a5d3    FastEthernet0/1'

    In [4]: re.search('(a1)+', line).group()
    Out[4]: 'a1a1'

    Expresson ``(a1)+`` uses brackets to specify that repetition is related to sequence of symbols 'a1'.

IP address can be described by ``\d+\.\d+\.\d+\.\d+``. This plus is used to indicate that there can be several digits. And there’s also an expression  ``\.``.

It is required because the dot is a special symbol (it denotes any symbol). And in order to indicate that we are interested in a dot as a symbol, you have to screen it - put a backslash in front of a dot.

Using this expression, you can get an IP address from sh_ip_int_br string:

.. code:: python

    In [5]: sh_ip_int_br = 'Ethernet0/1    192.168.200.1   YES NVRAM  up          up'

    In [6]: re.search('\d+\.\d+\.\d+\.\d+', sh_ip_int_br).group()
    Out[6]: '192.168.200.1'

Another example of an expression: ``\d+\s+\S+`` - describes a string which has digits first, then whitespace characters, and then non-whitespace characters (all except space, tab, and other similar characters).
Using it you can get VLAN and MAC address from string:

.. code:: py

    In [7]: line = '1500     aab1.a1a1.a5d3    FastEthernet0/1'

    In [8]: re.search('\d+\s+\S+', line).group()
    Out[8]: '1500     aab1.a1a1.a5d3'

``*``
~~~~~

Asterisk indicates that the previous expression can be repeated 0 or more times.

For example, if an asterisk stands after 'a' symbol, it means a repetition of that symbol.

Expression ``ba*`` means ‘b’ and then zero or more repetitions of ‘a’:

.. code:: py

    In [9]: line = '100     a011.baaa.a5d3    FastEthernet0/1'

    In [10]: re.search('ba*', line).group()
    Out[10]: 'baaa'

In *line* string, if ‘b’ meets before ‘baaa’ substring, then the match is ‘b’:

.. code:: py

    In [11]: line = '100     ab11.baaa.a5d3    FastEthernet0/1'

    In [12]: re.search('ba*', line).group()
    Out[12]: 'b'

Suppose you write a regular expression that describes email addresses in two formats: user@example.com and user.test@example.com. That is, the left side of address can have either one word or two words separated by a dot.

The first variant is an example of email without a dot:

.. code:: python

    In [13]: email1 = 'user1@gmail.com'

This address can be described by ``\w+@\w+\.\w+``:

.. code:: python

    In [14]: re.search('\w+@\w+\.\w+', email1).group()
    Out[14]: 'user1@gmail.com'

But such an expression is not suitable for an email address with a dot:

.. code:: python

    In [15]: email2 = 'user2.test@gmail.com'

    In [16]: re.search('\w+@\w+\.\w+', email2).group()
    Out[16]: 'test@gmail.com'

Regular expression for email with a dot:

.. code:: python

    In [17]: re.search('\w+\.\w+@\w+\.\w+', email2).group()
    Out[17]: 'user2.test@gmail.com'

To describe both email, you have to specify that the dot is optional:

::

    '\w+\.*\w+@\w+\.\w+'

This regular expression describes both options:

.. code:: python

    In [18]: email1 = 'user1@gmail.com'

    In [19]: email2 = 'user2.test@gmail.com'

    In [20]: re.search('\w+\.*\w+@\w+\.\w+', email1).group()
    Out[20]: 'user1@gmail.com'

    In [21]: re.search('\w+\.*\w+@\w+\.\w+', email2).group()
    Out[21]: 'user2.test@gmail.com'

``?``
~~~~~

In the last example, regular expression indicates that the dot is optional, but at the same time determines that it can appear many times.

In this situation, it is more logical to use a question mark. It denotes zero or one repetition of a preceding expression or symbol. Now regular expression looks like ``\w+\.?\w+@\w+\.\w+``:

.. code:: python

    In [22]: mail_log = ['Jun 18 14:10:35 client-ip=154.10.180.10 from=user1@gmail.com, size=551',
         ...:             'Jun 18 14:11:05 client-ip=150.10.180.10 from=user2.test@gmail.com, size=768']

    In [23]: for message in mail_log:
         ...:     match = re.search('\w+\.?\w+@\w+\.\w+', message)
         ...:     if match:
         ...:         print("Found email: ", match.group())
         ...:
    Found email:  user1@gmail.com
    Found email:  user2.test@gmail.com

``{n}``
~~~~~~~

You can set how many times the previous expression should be repeated with curly brackets.

For example, expression ``\w{4}\.\w{4}\.\w{4}`` describes 12 letters or digits that are divided into three groups of four characters and separated by dot. This way you can get a MAC address:

.. code:: py

    In [24]: line = '100     aab1.a1a1.a5d3    FastEthernet0/1'

    In [25]: re.search('\w{4}\.\w{4}\.\w{4}', line).group()
    Out[25]: 'aab1.a1a1.a5d3'

You can specify a repetition range in curly brackets. For example, try to get all VLAN numbers from string mac\_table:

.. code:: python

    In [26]: mac_table = '''
        ...: sw1#sh mac address-table
        ...:           Mac Address Table
        ...: -------------------------------------------
        ...:
        ...: Vlan    Mac Address       Type        Ports
        ...: ----    -----------       --------    -----
        ...:  100    a1b2.ac10.7000    DYNAMIC     Gi0/1
        ...:  200    a0d4.cb20.7000    DYNAMIC     Gi0/2
        ...:  300    acb4.cd30.7000    DYNAMIC     Gi0/3
        ...: 1100    a2bb.ec40.7000    DYNAMIC     Gi0/4
        ...:  500    aa4b.c550.7000    DYNAMIC     Gi0/5
        ...: 1200    a1bb.1c60.7000    DYNAMIC     Gi0/6
        ...: 1300    aa0b.cc70.7000    DYNAMIC     Gi0/7
        ...: '''

Since search() only looks for the first match, expression ``\d{1,4}`` 
will have VLAN number:

.. code:: python

    In [27]: for line in mac_table.split('\n'):
        ...:     match = re.search('\d{1,4}', line)
        ...:     if match:
        ...:         print('VLAN: ', match.group())
        ...:
    VLAN:  1
    VLAN:  100
    VLAN:  200
    VLAN:  300
    VLAN:  1100
    VLAN:  500
    VLAN:  1200
    VLAN:  1300

Expression ``\d{1,4}`` describes one to four digits.

Note that the output of command from equipment does not have a VLAN with number 1. Regular expression got a number 1 from somewhere. Number 1 was in the output from hostname in line ``sw1#sh mac address-table``.

To correct this, it suffices to complete an expression and indicate that at least one space must follow the numbers:

.. code:: python

    In [28]: for line in mac_table.split('\n'):
        ...:     match = re.search('\d{1,4} +', line)
        ...:     if match:
        ...:         print('VLAN: ', match.group())
        ...:
    VLAN:  100
    VLAN:  200
    VLAN:  300
    VLAN:  1100
    VLAN:  500
    VLAN:  1200
    VLAN:  1300

