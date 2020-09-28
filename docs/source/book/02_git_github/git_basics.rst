Git fundamentals
~~~~~~~~~~

Git is a distributed version control system (Version Control System, VCS) that is widely used and released under GNU GPL v2 license. 
It can:

-  track changes in files;
-  store multiple versions of the same file;
-  cancel changes made;
-  record who made changes and when.

Git stores changes as a snapshot of entire repository. This snapshot is created after each “commit” command.

Git installation:

::

    $ sudo apt-get install git

Git initial setup
^^^^^^^^^^^^^^^^^^^^^^^

To start working with Git you need to specify user name and e-mail that will be used to synchronize local repository with Github repository:

::

    $ git config --global user.name "username"
    $ git config --global user.email "username.user@example.com"

See Git settings:

::

    $ git config --list

Repository initialization
^^^^^^^^^^^^^^^^^^^^^^^^^

Repository is initialized using "git init" command:

::

    [~/tools/first_repo]
    $ git init
    Initialized empty Git repository in /home/vagrant/tools/first_repo/.git/

After executing this command, current directory creates .git folder containing service files needed for Git.
