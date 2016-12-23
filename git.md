# Git

## configuration

### Create SSH keys

1. Create a pair of SSH key.

    ```sh
    $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    Generating public/private rsa key pair.
    ```
2. Name your key based on its purpose (including `~/.ssh/` folder) and then enter a strong passphrase.

    ```sh
    Enter file in which to save the key (/home/you/.ssh/id_rsa):  [~/.ssh/<github-user>_id_rsa]
    ```

4. Configure `ssh-agent` to remember your passphrase:
    - **Linux**: Add the following snippet at the end of `.bashrc`.

        ```sh

        # Automatically start ssh-agent and load the ssh-key(s) on login:
        if [ -z "$SSH_AUTH_SOCK" ] ; then
          eval `ssh-agent -s`
          ssh-add
        fi
        ```
    - **MacOS X**: Add your key to _Keychain_.

        ```sh
        $ ssh-add -K ~/.ssh/<github-user>_id_rsa
        ```
5. Add github.com to SSH known host.

    ```sh
    $ ssh-keyscan -t rsa github.com > ~/.ssh/known_hosts
    ```

### SSH configuration for multiple GitHub account

**Reference:** Adapted from [Multiple SSH Keys settings for different github account](https://gist.github.com/jexchan/2351996).

1. Create one SSH key per GitHub account. You cannot use the same key for two different GitHub accounts.

    ```sh
    # Name your key id_<github-user>_rsa
    ssh-keygen -t rsa -b 4096 -C "your_email@youremail.com"
    ssh-add ~/.ssh/<github-user>_id_rsa
    # Under MacOS X you can even add it to your Keychain
    # ssh-add -K ~/.ssh/<github-user>_id_rsa
    ```
1. Create a `config` file in `~/.ssh/` folder and add the two GitHub accounts:

    ```
    # First account
    Host github.com-<github-user-1>
        HostName github.com
        User git
        IdentityFile ~/.ssh/<github-user-1>_id_rsa

    # Second account
    Host github.com-<github-user-2>
        HostName github.com
        User git
        IdentityFile ~/.ssh/<github-user-2>_id_rsa
    ```

1. Change the `[remote "origin"]` field in your local `.git/config` to use the Host defined in `.ssh/config` for each repository:

    ```
    [remote "origin"]
            url = git@github.com-<github-user>:<github-user>/<repo>.git
    ```

1. Don't forget to set `name` and `email` for your local repository configuration based on the GitHub user:

    ```sh
    $ git config user.name "Your Name"
    $ git config user.email "your_email@youremail.com"
    ```

$ ssh-add -D

finally, you can check your saved keys

$ ssh-add -l

Modify the ssh config

$ cd ~/.ssh/
$ touch config
$ subl -a config

Then added


Clone you repo and modify your Git config

clone your repo git clone git@github.com:activehacker/gfs.git gfs_jexchan

cd gfs_jexchan and modify git config

$ git config user.name "jexchan"
$ git config user.email "jexchan@gmail.com"

$ git config user.name "activehacker"
$ git config user.email "jexlab@gmail.com"

or you can have global git config $ git config --global user.name "jexchan" $ git config --global user.email "jexchan@gmail.com"

then use normal flow to push your code

$ git add .
$ git commit -m "your comments"
$ git push

Another related article in Chinese

    http://4simple.github.com/docs/multipleSSHkeys/

@oleweidner
oleweidner commented on 3 Sep 2012

I had to change the [remote "origin"] / url field in my local .git/config to use the Host defined in .ssh/config in order for this to work, i.e.,

[remote "origin"]
        url = git@github.com-activehacker:activehacker/gfs.git

Without that modification, git would just try to use my default ssh key.
