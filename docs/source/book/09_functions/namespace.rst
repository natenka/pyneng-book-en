Namespace. Scope of variables
------------------------------------

Variables in Python have a scope. Depending on location in code where
variable has been defined, scope is also defined, it determines where variable will be available.

When using variable names in a program, Python searches, creates or changes
these names in the corresponding namespace each time. Namespace that is available at each moment depends on area in which code is located.

Python has a LEGB rule that it uses for variables search.
For example, when accessing a variable within a function, Python searches for a variable in this order in scopes (before the first match):

* L (local) - in local (within function)
* E (enclosing) - in local area of outer functions (these are functions within which our function is located)
* G (global) - in global (in script)
* B (built-in) - in built-in (reserved Python values)

Accordingly, there are local and global variables:

* local variables:
  
  * variables that are defined within function
  * these variables become unavailable after exit from function

* global variables:
  
  * variables that are defined outside the function
  * these variables are 'global' only within a module
  * for example, to be available in another module they must be imported

Example of local intf_config:

.. code:: python

    In [1]: def configure_intf(intf_name, ip, mask):
       ...:     intf_config = f'interface {intf_name}\nip address {ip} {mask}'
       ...:     return intf_config
       ...:

    In [2]: intf_config
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    <ipython-input-2-5983e972fb1c> in <module>
    ----> 1 intf_config

    NameError: name 'intf_config' is not defined


Note that intf_config variable is not available outside of function. To get the
result of a function you must call a function and assign result to a variable:

.. code:: python

    In [3]: result = configure_intf('F0/0', '10.1.1.1', '255.255.255.0')

    In [4]: result
    Out[4]: 'interface F0/0\nip address 10.1.1.1 255.255.255.0'


