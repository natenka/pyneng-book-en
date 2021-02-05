.. raw:: latex

   \newpage

.. _git_github_index:

2. Using Git and Github
=============================

.. warning::

    This is a difficult section to understand. Especially because this section is at the very beginning of the book.
    You can skip it, but I would highly recommend trying to understand the basics of git/Github and use it to store tasks.


There are a lot of tasks in book and you have to store them somewhere. One
option is to use Git and Github to do this. Of course, there are other ways
to do this but Github can be used for other things in future.
Tasks and examples from book are in a separate `repository <https://github.com/natenka/pyneng-examples-exercises/>`__
on Github. They can be downloaded as a zip archive but it is better to work
with repository using Git, then you can see changes made and easily update
repository. If this is the first time working with Git and especially if this
is the first version control system you work with, there are a lot of
information, so this chapter focuses on practical side of question and it says:

-  How to start using Git and Github
-  How to perform basic setup
-  How to view information and/or changes

There will be no much theory in this subsection but references to useful
resources are listed. Try doing all basic settings for tasks and then continue
reading the book. And at the end, when basic work with Git and Github is already
routine, read more about them. What could Git be useful for:

-  to store configurations and all configuration changes
-  to store documentation and all its versions
-  to store schemes and all its versions
-  to store code and its versions

Github allows you to centrally store all above items, but it should be taken
into account that these resources will be available to others as well. Github
also has private repositories, but even these probably should not contain
information such as passwords. Of course, main use of Github is to place code
of various projects. In addition, Github can be also used to:

-  host for your website  (`GitHub
   Pages <https://pages.github.com/>`__)
-  Hosting for online presentations and a tool to create them
   (`GitPitch <https://gitpitch.com/>`__)
-  together with `GitBook <https://www.gitbook.com>`__, it is also a platform for publishing books, documentation, etc


.. toctree::
   :maxdepth: 1

   git_basics
   git_basics_bash_status
   git_basics_commands
   git_basics_additional
   git_github_auth
   git_github_changes
   pyneng_github
   further_reading
   ../../exercises/02_exercises.rst
