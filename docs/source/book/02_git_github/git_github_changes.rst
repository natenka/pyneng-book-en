Working with own repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This chapter describes how to work with repository on your local machine.

Creating a Github repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create a Github repository you need:

-  log in to `GitHub <https://github.com/>`__;
-  In upper right corner press plus and select "New repository" to create a new repository;
-  Name of repository should be entered in window that appears;

You can put "Initialize this repository with a README". This will create a README.md file that only contains repository name.

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/github_new_repo.png

Cloning a Github repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To work locally with repository, it should be cloned.

Use *git clone* command to clone repository:

::

    $ git clone ssh://git@github.com/pyneng/online-2-natasha-samoylenko.git
    Cloning into 'online-2-natasha-samoylenko'...
    remote: Counting objects: 241, done.
    remote: Compressing objects: 100% (191/191), done.
    remote: Total 241 (delta 43), reused 239 (delta 41), pack-reused 0
    Receiving objects: 100% (241/241), 119.60 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (43/43), done.
    Checking connectivity... done.

Compared to this command, you need to change:

-  'pyneng user name' for your Github user name;
-  'online-2-natasha-samoylenko' repository name for your Github repository.

As a result, in current directory in which *git clone* was executed, a directory with name of repository will appear, in my case - "online-2-natasha-samoylenko". This directory now contains the contents of Github repository.

Working with repository
^^^^^^^^^^^^^^^^^^^^^

The previous command not only copied repository to use it locally, but also configured Git accordingly:

-  Folder .git was created
-  All repository data is downloaded
-  Downloaded all changes that were in repository
-  Github repository is configured as a remote for local repository

Now you have a complete local Git repository where you can work. Typically, sequence of steps will be as follows:

-  Before starting, synchronize local content with Github using *git pull* command
-  Modifying repository files
-  Adding modified files to staging with “git add” command
-  Commit changes using *git commit* command
-  Transferring local changes to Github repository with *git push* command

When working with tasks at work and at home, it is necessary to pay special attention to first and last step:

-  The first step is to update local repository
-  The last step - load changes to Github

Synchronizing local repository with remote repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All commands are executed inside repository directory (in example above - online-2-natasha-samoylenko).

If contents of local repository are the same as those of remote repository, output will be:

::

    $ git pull
    Already up-to-date.

If there were changes, output would be something like this:

::

    $ git pull
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (1/1), done.
    remote: Total 5 (delta 4), reused 5 (delta 4), pack-reused 0
    Unpacking objects: 100% (5/5), done.
    From ssh://github.com/pyneng/online-2-natasha-samoylenko
       89c04b6..fc4c721  master     -> origin/master
    Updating 89c04b6..fc4c721
    Fast-forward
     exercises/03_data_structures/task_3_3.py | 2 ++
     1 file changed, 2 insertions(+)

Adding new files or changes to existing files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you want to add a specific file (in this case, README.md), you need to enter ``git add README.md`` command. All files of current directory are added by ``git add .`` command.

Commit
^^^^^^

You should specify message when you are running a commit. It is better if message is with meaning, rather than just "update" or similar. Commit could be done by a command similar to ``git commit -m "Tasks 4.1-4.3 are completed"``.

Push on GitHub
^^^^^^^^^^^^^^

Command “git push” is used to load all local changes to Github:

::

    $ git push origin master
    Counting objects: 5, done.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (5/5), 426 bytes | 0 bytes/s, done.
    Total 5 (delta 4), reused 0 (delta 0)
    remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
    To ssh://git@github.com/pyneng/online-2-natasha-samoylenko.git
       fc4c721..edcf417  master -> master

Before executing “git push” you can run ``git log -p/origin..`` - it will show what changes you are going to add to your repository on Github.
