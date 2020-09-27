.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 11.1
~~~~~~~~~~~~

Create a parse_cdp_neighbors() function that handles the output of show cdp neighbors command.

Function should have one parameter -  command_output, that expects as an argument the command output as a single string (not file name). To do this, you should read the entire contents of file into a string and then pass string as function argument (how to pass command output is shown in code below).

Function should return a dictionary that describes connections between devices.

For example, if such an output is given as an argument:

::

    R4>show cdp neighbors

    Device ID    Local Intrfce   Holdtme     Capability       Platform    Port ID
    R5           Fa 0/1          122           R S I           2811       Fa 0/1
    R6           Fa 0/2          143           R S I           2811       Fa 0/0

Function should return such dictionary:

.. code:: python

    {("R4", "Fa0/1"): ("R5", "Fa0/1"),
     ("R4", "Fa0/2"): ("R6", "Fa0/0")}

In dictionary, interfaces should be written without space between type and name. That is Fa0/0, not Fa 0/0.

Check function with contents of sh_cdp_n_sw1.txt file. Function also should work on other files (test checks function operation on output from sh_cdp_n_sw1.txt and sh_cdp_n_r3.txt).

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    def parse_cdp_neighbors(command_output):
        """
        Here we pass command output with one string because in this form we will receive command output from equipment. 
        Taking command output as argument, instead of a file name, we make function more universal: 
        it can work with both files and output from equipment. Plus, we learn to work with that output.
        """


    if __name__ == "__main__":
        with open("sh_cdp_n_sw1.txt") as f:
            print(parse_cdp_neighbors(f.read()))


Task 11.2
~~~~~~~~~~~~

Create a create_network_map() function that handles the output of show cdp neighbors command from multiple files and integrates it into one common topology.

Function should have one parameter – filenames, that expects as an argument a list of file names in which show cdp neighbors output is found.

Function should return a dictionary that describes connections between devices. Structure of dictionary is the same as in task 11.1:

.. code:: python

    {("R4", "Fa0/1"): ("R5", "Fa0/1"),
     ("R4", "Fa0/2"): ("R6", "Fa0/0")}


Generate a topology that matches the output from files:

* sh_cdp_n_sw1.txt
* sh_cdp_n_r1.txt
* sh_cdp_n_r2.txt
* sh_cdp_n_r3.txt

There should be no duplicates in dictionary that returns create_network_map() function.

Using draw_topology() function from draw_network_graph.py file, draw a diagram based on topology received with create_network_map() function. The result should look the same as scheme in task_11_2_topology.svg file

.. figure:: https://raw.githubusercontent.com/natenka/pyneng-examples-exercises/master/exercises/11_modules/task_11_2_topology.png

At the same time:

* The arrangement of devices on diagram may be different
* Connections should follow the diagram

Do not copy code of functions parse_cdp_neighbors() and draw_topology().

Restriction: All tasks must be performed using only covered topics.

.. note::
    To complete this task, graphviz must be installed: :
    ``apt-get install graphviz``

    And a python module for working with graphviz:
    ``pip install graphviz``


.. code:: python

    # These blanks are written to show at what moment
    # a topology should be drawn (after function call)
    def create_network_map(filenames):
        pass


    if __name__ == "__main__":
        infiles = [
            "sh_cdp_n_sw1.txt",
            "sh_cdp_n_r1.txt",
            "sh_cdp_n_r2.txt",
            "sh_cdp_n_r3.txt",
        ]

        topology = create_network_map(infiles)
        # draw topology:
        # draw_topology(topology)

