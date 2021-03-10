.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 11.1
~~~~~~~~~~~~

Create a function parse_cdp_neighbors that handles
show cdp neighbors command output.

The function must have one parameter, command_output, which expects a single
string of command output as an argument (not a filename). To do this, you need
to read the entire contents of the file into a string, and then pass the string
as an argument to the function (how to pass the command output is shown in the code below).

The function should return a dictionary that describes the connections between devices.


For example, if the following output was passed as an argument:

::

    R4>show cdp neighbors

    Device ID    Local Intrfce   Holdtme     Capability       Platform    Port ID
    R5           Fa 0/1          122           R S I           2811       Fa 0/1
    R6           Fa 0/2          143           R S I           2811       Fa 0/0

Function should return a dictionary:

.. code:: python

    {("R4", "Fa0/1"): ("R5", "Fa0/1"),
     ("R4", "Fa0/2"): ("R6", "Fa0/0")}

In the dictionary, interfaces must be written without a space between type and name.
That is, so Fa0/0, and not so Fa 0/0.

Check the operation of the function on the contents of the sh_cdp_n_sw1.txt file.
In this case, the function should work on other files (the test checks the operation
of the function on the output from sh_cdp_n_sw1.txt and sh_cdp_n_r3.txt).

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    def parse_cdp_neighbors(command_output):
        """
        Here we pass the output of the command as single string because it is in this
        form that received command output from equipment. Taking the output of
        the command as an argument, instead of a filename, we make the function more
        generic: it can work both with files and with output from equipment.
        Plus, we learn to work with such a output.
        """


    if __name__ == "__main__":
        with open("sh_cdp_n_sw1.txt") as f:
            print(parse_cdp_neighbors(f.read()))


Task 11.2
~~~~~~~~~~~~

Create a create_network_map function that processes the show cdp neighbors
command output from multiple files and merges it into one common topology.

The function must have one parameter, filenames, which expects as an argument
a list of filenames containing the output of the show cdp neighbors command.

The function should return a dictionary that describes the connections
between devices. The structure of the dictionary is the same as in task 11.1:

.. code:: python

    {("R4", "Fa0/1"): ("R5", "Fa0/1"),
     ("R4", "Fa0/2"): ("R6", "Fa0/0")}


Generate topology that matches the output from the files:

* sh_cdp_n_sw1.txt
* sh_cdp_n_r1.txt
* sh_cdp_n_r2.txt
* sh_cdp_n_r3.txt

Do not copy the code of the parse_cdp_neighbors and draw_topology functions.
If the parse_cdp_neighbors function cannot process the output of one of the
command output files, you need to correct the function code in task 11.1.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    infiles = [
        "sh_cdp_n_sw1.txt",
        "sh_cdp_n_r1.txt",
        "sh_cdp_n_r2.txt",
        "sh_cdp_n_r3.txt",
    ]


Task 11.2a
~~~~~~~~~~~~

.. note::

    To complete this task, graphviz must be installed:
    apt-get install graphviz

    And a python module to work with graphviz:
    pip install graphviz

Use the create_network_map function from task 11.2 to create the topology dict
for files:

* sh_cdp_n_sw1.txt
* sh_cdp_n_r1.txt
* sh_cdp_n_r2.txt
* sh_cdp_n_r3.txt

Using the draw_topology function from the draw_network_graph.py file, draw
schema for the topology dict received with create_network_map function.
You need to figure out how to work with the draw_topology function on your own,
by reading the function description in the draw_network_graph.py file.
The resulting scheme will be written to the svg file - it can be opened with a browser.


With the current topology dictionary, extra connections are drawn on the diagram. They
occur because one CDP file (sh_cdp_n_r1.txt) describes connection
``("R1", "Eth0/0"): ("SW1", "Eth0/1")`` and another (sh_cdp_n_sw1.txt) describes
connection ``("SW1", "Eth0/1"): ("R1", "Eth0/0")``.

In this task, you need to create a new function unique_network_map, which of these
two connections will leave only one, for correct drawing of the schema.
In this case, it does not matter which of the connections to leave.

The unique_network_map function must have one topology_dict parameter,
which expects a dictionary as an argument.
It should be a dictionary resulting from the create_network_map function call.

Dict example:

.. code:: python

    {
        ("R1", "Eth0/0"): ("SW1", "Eth0/1"),
        ("R2", "Eth0/0"): ("SW1", "Eth0/2"),
        ("R2", "Eth0/1"): ("SW2", "Eth0/11"),
        ("R3", "Eth0/0"): ("SW1", "Eth0/3"),
        ("R3", "Eth0/1"): ("R4", "Eth0/0"),
        ("R3", "Eth0/2"): ("R5", "Eth0/0"),
        ("SW1", "Eth0/1"): ("R1", "Eth0/0"),
        ("SW1", "Eth0/2"): ("R2", "Eth0/0"),
        ("SW1", "Eth0/3"): ("R3", "Eth0/0"),
        ("SW1", "Eth0/5"): ("R6", "Eth0/1"),
    }


The function should return a dictionary that describes the connections between
devices. In the dictionary, you need to get rid of "duplicate" connections and
leave only one of them.

The structure of the final dict is the same as in task 11.2:

.. code:: python

    {("R4", "Fa0/1"): ("R5", "Fa0/1"),
     ("R4", "Fa0/2"): ("R6", "Fa0/0")}

After creating the function, try drawing the topology again,
now for the dictionary returned by the unique_network_map function.

The result should look the same as the diagram in task_11_2a_topology.svg

.. figure:: https://raw.githubusercontent.com/natenka/pyneng-examples-exercises-en/master/exercises/11_modules/task_11_2a_topology.png


Wherein:

* The arrangement of devices on the diagram may be different
* Connections must match the diagram

Do not copy the code of the create_network_map and draw_topology functions.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

.. code:: python

    infiles = [
        "sh_cdp_n_sw1.txt",
        "sh_cdp_n_r1.txt",
        "sh_cdp_n_r2.txt",
        "sh_cdp_n_r3.txt",
    ]
