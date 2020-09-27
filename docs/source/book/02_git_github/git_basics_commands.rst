Working with Git
^^^^^^^^^^^^

Various commands are used to control Git, the meaning of which is explained below.

git status
''''''''''

When working with Git it is important to understand the current status of the repository. For this purpose Git has a “git status” command


.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_0.png

Git reports that we are in the master branch (this branch is auto-created and used by default) and that it has nothing to commit. Git also offers to create or copy files and then use the “git add” command to start Git tracking them.

Create README file and add "test" line to it

::

    $ vi README
    $ echo "test" >> README

After that, the invitation looks like this

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/bash_prompt.png

The invitation shows that there are two files that Git is not following

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_1.png

Two files came out because I have undo-files configured for Vim. These are special files that allow you to cancel changes not only in the current file session but also in the past. Note that Git reports there are files that it does not follow and tells you using which command you can start following.

File .gitignore
'''''''''''''''

Undo-file .README.un~ is a service file that does not need to be added to repository. Git has the option to specify which files or directories to ignore. To do this, you need to create appropriate templates in the . gitignore file in the repository directory.

To make Git ignore undo-files of Vim you can add such a line to the file .gitignore

::

    *.un~

This means that Git must ignore all files that end with ".un~".

After that, git status shows

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_2.png

Note that there is no .README.un~ file in the output. Once a file was added to the repository .gitignore, the files that are listed in it are being ignored.

git add
'''''''

The “git add” command is used to start Git following files.

You can specify that you want to follow a particular file

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_add_readme.png

Or all the files

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_add_all.png

Git status output

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_3.png

Now the files are in a section called "Changes to be committed".

git commit
''''''''''

After all the necessary files have been added in staging, you can commit the changes. Staging is a collection of files that will be added to the next commit. The "git commit" command has only one obligatory parameter - the flag "-m". It allows you to specify a message for this commit.

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_commit_1.png

After that, git status displays

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_4.png

The phrase "nothing to commit, working directory clean" indicates that there are no changes to add to Git or to commit.
