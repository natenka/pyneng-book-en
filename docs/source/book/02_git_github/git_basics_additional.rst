Additional facilities
^^^^^^^^^^^^^^^^^^^^^^^^^^

git diff
''''''''

The command "git diff" allows you to see the difference between different states. For example, README and .gitignore files have been changed in repository.

The "git status" command shows that both files have been changed

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_status_5.png

The “git diff” command shows what changes have been made since the last commit

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_diff.png

If you add changes made to staging via “git add” command and run “git diff” again, it will show nothing

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_add_git_diff.png

To show the difference between staging and the last commit, add parameter ``--staged``

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_diff_staged.png

Commit the changes

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_commit_2.png

git log
'''''''

The “git log” command shows when the last changes were made

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_log.png

By default, the command displays all commits starting from the nearest time. With the help of additional parameters it is possible not only to look at the information about commits but also what changes have been made.

The ``-p`` flag allows you to display the differences that have been made by each commit

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_log_p.png

Shorter output option can be displayed with flag ``--stat``

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/git_log_stat.png


