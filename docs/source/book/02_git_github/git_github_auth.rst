Github authentication
~~~~~~~~~~~~~~~~~~~~~~~~

To start working with Github you must 
`register <https://github.com/join>`__ on it. It is better to use SSH key authentication to work safely with Github.


Generation of a new SSH key (use e-mail that is linked to Github):

::

    $ ssh-keygen -t rsa -b 4096 -C "github_email@gmail.com"

All questions need to be pressed Enter (it is more secure to use the passphrase key but it is possible without it, if you press Enter when asked then passphrase will not be requested from you permanently during operations with the repository).

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

After copying, go to Github. When you are on any Github page, in the upper right-hand corner click on the picture of your profile and select "Settings" in the drop down list. In the list on the left, select the field "SSH and GPG keys". Then press "New SSH key" and in the field "Title" write the key name (for example "Home") and in the field "Key" insert the content that was copied from the file ~/. ssh/id_rsa.pub.

.. note::
    If Github requests a password, enter your account password on Github.

To check if everything has been successful, try executing the command
``ssh -T git@github.com``.

The output should be as follows:

::

    $ ssh -T git@github.com
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.

Now you are ready to work with Git and Github.
