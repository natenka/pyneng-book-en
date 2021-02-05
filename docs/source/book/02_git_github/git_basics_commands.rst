Working with Git
^^^^^^^^^^^^

There are a few basic commands you need to know to work with Git.
 
git status
''''''''''

When working with Git it is important to understand current status of repository. For this purpose Git has a ``git status`` command


.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_0.png

Git reports that we are in master branch (this branch is auto-created and used
by default) and that it has nothing to commit. Git also offers to create or
copy files and then use ``git add`` command to start Git tracking them.

Create README file and add "test" line to it

::

    $ vi README
    $ echo "test" >> README

After that, invitation looks like this

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/bash_prompt.png

Invitation shows that there are two untracked files:

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_1.png

Two files came out because I have undo-files configured for Vim. These are special files that allow you
to undo changes not only in current file session but also in the previous sessions. Note that Git reports 
there are files that it does not track and tells you using which command you can start tracking.


File .gitignore
'''''''''''''''

Undo-file .README.un~ is a special file that does not need to be added to
repository. Git has option  to specify which files or directories to ignore.
To do this, you need to create appropriate templates in ``.gitignore`` file in repository directory.

To make Git ignore Vim undo-files you can add such a line to .gitignore file

::

    *.un~

This means that Git must ignore all files that end with ".un~".

After that, git status shows

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_2.png

Note that there is no .README.un~ file in the output. Once a file was added to
repository .gitignore, files that are listed in it are being ignored.

git add
'''''''

Command ``git add`` is used to start Git tracking files.

You can specify that you want to track a particular file

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_add_readme.png

Or all files

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_add_all.png

Git status output

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_3.png

Now files are in a section called "Changes to be committed".

git commit
''''''''''

After all necessary files have been added in staging, you can commit changes.
Staging is a collection of files that  will be added to the next commit.
Command ``git commit`` has only one mandatory parameter - flag "-m".
It allows you to specify a message for this commit.

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_commit_1.png

After that, git status displays

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_4.png

Phrase "nothing to commit, working directory clean" indicates that there are
no changes to add to Git or to commit.
