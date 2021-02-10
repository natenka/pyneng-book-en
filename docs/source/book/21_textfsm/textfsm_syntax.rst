TextFSM template syntax
--------------------------

This section covers template syntax based on TextFSM documentation. The
following section shows examples of syntax usage. Therefore, you can move
to the next section and  return to this section as necessary for those
situations for which there is no example and when you need to recall
the meaning of parameter.

TextFSM template describes how data should be processed.
Each template consists of two parts:

* value definitions (or variable definitions) - these variables describe which columns will be in the table view 
* state definitions

Example of traceroute command examination:

::

    Value ID (\d+)
    Value Hop (\d+(\.\d+){3})

    Start
       ^  ${ID} ${Hop} -> Record

Value definition
~~~~~~~~~~~~~~~~~~~~~~

Only value definitions should be used in value section. The only exception
there may be comments in this section.
This section should not contain empty strings. For TextFSM, an empty string
means the end of value definition section.

Format of value description:

::

    Value [option[,option...]] name regex

Syntax of value description (for each option below we will consider examples):

* ``Value`` - keyword that indicates that a value is being created. It must be specified
* option - options that define how to work with a variable. If several options
  are to be specified, they must be separated by a comma, without spaces. These options are supported:

  * ``Filldown`` - value that previously matched with a regex,  remembered until
    the next processing line (if has not been explicitly cleared or matched again).
    This means that the last column value that matches regex is stored and used
    in the following strings if this column is not present.
  * ``Key`` - determines that this field contains a unique string identifier
  * ``Required`` - string that is processed will only be written if this value is present.
  * ``List`` - value is a list, and each match with a regex will add an item to the list.
    By default, each next match overwrites the previous one. 
  * ``Fillup`` - works as Filldown but fills empty value up until it finds a match. Not compatible with Required.

* ``name`` - name of value that will be used as column name. Reserved names should not be used as value names. 
* ``regex`` - regex that describes a value. regex should be in curly braces.

State definition
~~~~~~~~~~~~~~~~~~~~~

After defining values, we need to defiine states:

* each state definition must be separated by an empty line (at least one)
* first line - state name 
* then follows lines that describe rules. Rules must start with two spaces and caret symbol ``^``

Initial state is always ``Start``. Input data is compared to the current state
but rule line can specify that you want to go to a different state.

Checking is done line-by-line until ``EOF`` (end of file) is reached or the current state goes to ``End`` state.

Reserved states
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following states are reserved:

* ``Start`` - state that must be specified. Without it the template won't work.
* ``End`` - state that completes processing of incoming strings and does not execute ``EOF`` state. 
* ``EOF`` - implicit state that always executes when processing reaches the end of the file. It looks like this:

::

     EOF
       ^.* -> Record

``EOF`` writes down the current string before it is finished. If this behavior
needs to be changed you should explicitly write EOF at the end of template:

::

    EOF

State rules
-----------------

Each state consists of one or more rules: 

* TextFSM handles incoming strings and compares them to rules 
* if rule (regex) matches the string, actions in rule are executed and for the
  next string the process is repeated from the beginning of state.

Rules should be written in this format:

::

      ^regex [-> action]

In rule: 

* each rule must start with two spaces and caret symbol ``^``. Caret symbol
  ``^`` means the beginning of a line and must always be clearly indicated
* regex - regex in which variables can be used

  * to specify variable you can use syntax like ``$ValueName`` or ``${ValueName}``\ (preferred format) 
  * in rule, variables are substituted by regex 
  * if you want to explicitly specify end of line, use ``$$``

Action in rules
~~~~~~~~~~~~~~~~~~~

After regex, rule may indicate actions: 

* there must be ``->`` character between regex and action  
* actions can consist of three parts in such format:  ``L.R S`` 

  * ``L - Line Action`` - actions that apply to an input string
  * ``R - Record Action`` - actions that apply to collected values
  * ``S - State Transition`` - changing state

* By default ``Next.NoRecord`` is used

Line Actions
^^^^^^^^^^^^

Line Actions:

* ``Next`` - process the line, read the next line and start checking it from
  the beginning. This action is used by default unless otherwise specified
* ``Continue`` - continue to process rules as if there was no match while values are assigned

Record Action
^^^^^^^^^^^^^

``Record Action`` - optional action that can be specified after Line Action.
They must be separated by a dot. Types of actions:

* ``NoRecord`` - do nothing. This is default action when no other is specified
* ``Record`` - all variables except those with Filldown option are reset.
* ``Clear`` - reset all variables except those where Filldown option is specified.
* ``Clearall`` - reset all variables.

You need to split actions with a dot only if you want to specify both Line and
Record actions. If you need to specify only one of them, dot is not required.

State Transition
^^^^^^^^^^^^^^^^

A new state can be specified after action: 

* state must be one of reserved or defined in template
* if input line matches:

  * all actions are executed, 
  * the next line is read, 
  * then the current state changes to a new state and processing continues in new state.

If rule uses ``Continue`` action, it is not possible to change state inside
this rule. This rule is needed to avoid loops in sequence of states.

Error Action
^^^^^^^^^^^^

``Error`` stops all line processing, discards all lines that have been collected so far and returns an exception.

Syntax of this action is:

::

    ^regex -> Error [word|"string"]

