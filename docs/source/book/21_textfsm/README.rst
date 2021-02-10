Getting started with TextFSM
=======================

First, library must be installed:

::

    pip install textfsm

To use TextFSM you should create a template to handle the output of command.

Example of traceroute command output:

::

    r2#traceroute 90.0.0.9 source 33.0.0.2
    traceroute 90.0.0.9 source 33.0.0.2
    Type escape sequence to abort.
    Tracing the route to 90.0.0.9
    VRF info: (vrf in name/id, vrf out name/id)
      1 10.0.12.1 1 msec 0 msec 0 msec
      2 15.0.0.5  0 msec 5 msec 4 msec
      3 57.0.0.7  4 msec 1 msec 4 msec
      4 79.0.0.9  4 msec *  1 msec

For example, you have to get hops that packet went through.

In this case TextFSM template will look like this (traceroute.template file):

::

    Value ID (\d+)
    Value Hop (\S+)

    Start
      ^  ${ID} ${Hop} +\d+ -> Record

First two lines define variables:

* ``Value ID (\d+)`` - this line defines an ``ID`` variable that describes a
  regular expression: ``(\d+)`` - one or more digits, here are the hop numbers
* ``Value Hop (\S+)`` - line that defines a ``Hop`` variable that
  describes an IP address by such regular expression

After ``Start`` line, the template itself begins. In this case, it's very simple:

* ``^  ${ID} ${Hop} -> Record`` 
* first goes caret sign, then two spaces and ``ID`` and ``Hop`` variables
* in TextFSM the variables are written like this: ``${variable name}`` 
* word ``Record`` at the end means that lines that matches regular expression will be processed and included in the results of TextFSM (we'll look at this in the next section)

Script to process output of traceroute command with TextFSM (parse_traceroute.py):

.. code:: python

    import textfsm

    traceroute = '''
    r2#traceroute 90.0.0.9 source 33.0.0.2
    traceroute 90.0.0.9 source 33.0.0.2
    Type escape sequence to abort.
    Tracing the route to 90.0.0.9
    VRF info: (vrf in name/id, vrf out name/id)
      1 10.0.12.1 1 msec 0 msec 0 msec
      2 15.0.0.5  0 msec 5 msec 4 msec
      3 57.0.0.7  4 msec 1 msec 4 msec
      4 79.0.0.9  4 msec *  1 msec
    '''

    with open('traceroute.template') as template:
        fsm = textfsm.TextFSM(template)
        result = fsm.ParseText(traceroute)

    print(fsm.header)
    print(result)

The result of script execution:

::

    $ python parse_traceroute.py
    ['ID', 'Hop']
    [['1', '10.0.12.1'], ['2', '15.0.0.5'], ['3', '57.0.0.7'], ['4', '79.0.0.9']]

Lines that match the described template are returned as a list of lists. Each
element is a list that consists of two elements: hop number and IP address.

Let's sort out the content of script:

* traceroute - a variable that contains traceroute command output 
* ``template = open('traceroute.template')`` - content of TextFSM template file is read into a *template* variable
* ``fsm = textfsm.TextFSM(template)`` - class that processes a template and creates an object from it in TextFSM
* ``result = fsm.ParseText(traceroute)`` - method that handles output according to a template and returns a list of lists in which each element is a processed string 
* At the end, ``print(fsm.header)`` header is displayed which contains variable names and processing result

We can work with that output further. For example, periodically execute
traceroute command and compare whether the number of hops and their order has changed.

To work with TextFSM you need command output and template: 

* different commands need different templates 
* TextFSM returns a tabular processing result (as a list of lists)
* this output is easily converted to csv format or to a list of dictionaries

