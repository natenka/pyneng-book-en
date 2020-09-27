III. Regular expressions
#########################

A regular expression is a sequence of ordinary and special characters. This sequence specifies the template that is later used to find search pattern.

When working with network equipment, regular expressions can be used, for example, to:

* retrieve information from show command output
* select a portion of the lines from the show command output that matches the template
* check whether there are certain settings in configuration

A few examples are:

* After processing the output of “show version” command, you can collect information about OS version and uptime.
* get from the log file the lines that correspond to the template.
* get from the configuration those interfaces that do not have a description

In addition, in network equipment the regular expressions can be used to filter the output of any show commands.

In general, the use of regular expressions will involve getting part of the text out of a large output. But that’s not the only thing they can be used for. For example, regular expressions can be used to perform string replacements or for dividing a string.

These areas of use overlap with the methods that apply to strings. And if the problem is clear and simple to solve with string methods, it is better to use them. This code will be easier to understand and, in addition, string methods work faster.

But string methods may not solve all the problems or may make the problem much harder to solve. Regular expressions can help in this case.

.. toctree::
   :maxdepth: 1

   14_regex/index.rst
   15_module_re/index.rst
