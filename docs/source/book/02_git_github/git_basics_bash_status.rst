Displaying repository status in invitation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is an additional functionality that is not required to work with Git but is very helpful in this regard. When working with Git it is very convenient when you can immediately determine whether you are in a regular directory or in a Git repository. In addition, it would be good to understand the status of the current repository. To do this, you need to install a special
`utility  <https://github.com/magicmonty/bash-git-prompt/>`__ that will show the status of the repository. To install the utility, copy its repository to the user’s home directory under which you work:

.. code:: shell

    cd ~
    git clone https://github.com/magicmonty/bash-git-prompt.git .bash-git-prompt --depth=1

And then add to the end of .bashrc file such lines:

.. code:: shell

    GIT_PROMPT_ONLY_IN_REPO=1
    source ~/.bash-git-prompt/gitprompt.sh

To apply the changes, restart bash:

.. code:: shell

    exec bash

In my configuration the command line invitation is spread over several lines, so you will have a different one. Please note that additional information appears when you move to the repository.

Now, if you’re in a regular catalog, the invitation is like this:

::

    [~]
    vagrant@jessie-i386:
    $ 

If you go to the Git repository:

.. figure:: https://raw.githubusercontent.com/natenka/PyNEng/master/images/git/setup_prompt.png


