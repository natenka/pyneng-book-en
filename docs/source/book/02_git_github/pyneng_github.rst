Working with repository of tasks and examples
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All the examples and tasks of the book are published in a separate 
`repository <https://github.com/natenka/pyneng-examples-exercises>`__.

Copying repository from Github
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Examples and tasks are sometimes updated, so it will be more convenient to clone this repository to your machine and periodically update it.

To copy a repository from Github, run “git clone”:

::

    $ git clone https://github.com/natenka/pyneng-examples-exercises
    Cloning into 'pyneng-examples-exercises'...
    remote: Counting objects: 1263, done.
    remote: Compressing objects: 100% (504/504), done.
    remote: Total 1263 (delta 735), reused 1263 (delta 735), pack-reused 0
    Receiving objects: 100% (1263/1263), 267.10 KiB | 444.00 KiB/s, done.
    Resolving deltas: 100% (735/735), done.
    Checking connectivity... done.

Updating local copy of repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you need to update the local repository to synchronize it with Github version, you need to perform “git pull” from inside the created pyneng-examples-exercises directory.

If there were no updates, the output would be:

::

    $ git pull
    Already up-to-date.

If there were updates, the output would be something like this:

::

    $ git pull
    remote: Counting objects: 3, done.
    remote: Compressing objects: 100% (1/1), done.
    remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From https://github.com/natenka/pyneng-examples-exercises
       49e9f1b..1eb82ad  master     -> origin/master
    Updating 49e9f1b..1eb82ad
    Fast-forward
     README.md | 2 +-
     1 file changed, 1 insertion(+), 1 deletion(-)

Please note that only README.md file has been changed.

View changes
^^^^^^^^^^^^^^^^^^

If you want to see what changes have been made, you can use “git log”:

::

    $ git log -p -1
    commit 98e393c27e7aae4b41878d9d979c7587bfeb24b4
    Author: Наташа Самойленко <nataliya.samoylenko@gmail.com>
    Date:   Fri Aug 18 17:32:07 2017 +0300

        Update task_24_4.md

    diff --git a/exercises/24_ansible_for_network/task_24_4.md b/exercises/24_ansible_for_network/task_24_4.md
    index c4307fa..137a221 100644
    --- a/exercises/24_ansible_for_network/task_24_4.md
    +++ b/exercises/24_ansible_for_network/task_24_4.md
    @@ -13,11 +13,12 @@
     * apply ACL to interface

     ACL should be like:  
    +
     ip access-list extended INET-to-LAN
      permit tcp 10.0.1.0 0.0.0.255 any eq www
      permit tcp 10.0.1.0 0.0.0.255 any eq 22
      permit icmp any any
    -
    +

     Check playbook execution on the R1 router.

In this command ``-p`` flag indicates that the output of the Linux diff utility should be displayed for changes, not just the commit comment. In turn, ``-1`` indicates that only the latest commit should be shown.

View changes that will be synchronized
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The previous version of “git log” relies on the number of commands but this is not always convenient. Before executing “git pull” you can see what changes have been made since the last synchronization.

The following command shall be used:

::

    $ git log -p ..origin/master
    commit 4c1821030d20b3682b67caf362fd777d098d9126
    Author: Наташа Самойленко <nataliya.samoylenko@gmail.com>
    Date:   Mon May 29 07:53:45 2017 +0300

    Update README.md

    diff --git a/tools/README.md b/tools/README.md
    index 2b6f380..4f8d4af 100644
    --- a/tools/README.md
    +++ b/tools/README.md
    @@ -1 +1,4 @@
    +
    +Here you can find the PDF versions of configuration manuals of the tools that are used on the course.

In this case, the changes were in only one file. This command will be very useful to see what changes have been made to the tasks and which tasks. This will make it easier to navigate and to understand whether it is related to tasks you have already done and, if so, whether they should be changed.

.. note::
    "..origin/master" in ``git log -p ..origin/master``
    means to show all commits that are present in origin/master
    (in this case, it's GitHub) but that are not in the local copy of the repository

If the changes were in tasks you haven’t yet done, this output will tell you which files should be copied from the course repository to your personal repository (and maybe the entire section if you haven’t yet done the tasks from this section).
