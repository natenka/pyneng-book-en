Git fundamentals
~~~~~~~~~~

Git is a distributed version control system (Version Control System, VCS) that is widely used and released under the GNU GPL v2 license. 
It can:

-  track changes in files;
-  store multiple versions of the same file;
-  cancel the changes made;
-  record who made the changes and when.

Git stores the changes as a snapshot of the entire repository. This snapshot is created after each “commit” command.

Git installation:

::

    $ sudo apt-get install git

Git initial setup
^^^^^^^^^^^^^^^^^^^^^^^

To start working with Git you need to specify the user name and e-mail that will be used to synchronize the local repository with the Github repository:

::

    $ git config --global user.name "username"
    $ git config --global user.email "username.user@example.com"

See the Git settings:

::

    $ git config --list

Repository initialization
^^^^^^^^^^^^^^^^^^^^^^^^^

The repository is initialized using the "git init" command:

::

    [~/tools/first_repo]
    $ git init
    Initialized empty Git repository in /home/vagrant/tools/first_repo/.git/

After executing this command, the current directory creates .git folder containing the service files needed for Git.
