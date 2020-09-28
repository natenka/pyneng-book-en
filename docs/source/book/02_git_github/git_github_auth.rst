Github authentication
~~~~~~~~~~~~~~~~~~~~~~~~

To start working with Github you should 
`register <https://github.com/join>`__ on it. It is better to use SSH key authentication to work safely with Github.


Generation of a new SSH key (use e-mail that is linked to Github):

::

    $ ssh-keygen -t rsa -b 4096 -C "github_email@gmail.com"

All questions need to be pressed Enter (it is more secure to use passphrase key but it is possible without it, if you press Enter when asked then passphrase will not be requested from you permanently during operations with repository).

Start SSH agent:

::

    $ eval "$(ssh-agent -s)"

Add key to SSH agent:

::

    $ ssh-add ~/.ssh/id_rsa

Add SSH key to Github
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To add a key you have to copy it.

For example, you can display key to copy it:

::

    $ cat ~/.ssh/id_rsa.pub

After copying, go to Github. When you are on any Github page, in upper right-hand corner click on picture of your profile and select "Settings" in drop down list. In list on the left, select field "SSH and GPG keys". Then press "New SSH key" and in "Title" field write key name (for example "Home") and in field "Key" insert the content that was copied from file ~/. ssh/id_rsa.pub.

.. note::
    If Github requests a password, enter your account password on Github.

To check if everything has been successful, try executing command
``ssh -T git@github.com``.

The output should be as follows:

::

    $ ssh -T git@github.com
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.

Now you are ready to work with Git and Github.
