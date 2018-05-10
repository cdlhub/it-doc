# Git

## Git Prompt

https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh

1. Download `git-prompt.sh`:

    ```sh
    curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -o ~/.git-prompt.sh
    ```
2. Add the following lines to `.bash_profile`:

    ```sh
    if [ -f ~/.git-prompt.sh ]; then
        source ~/.git-prompt.sh
    else
        echo "ERROR: First run 'curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -o ~/.git-prompt.sh'"
    fi
    ```

## Git Completion

https://github.com/git/git/blob/master/contrib/completion/git-completion.bash

1. Download `git-completion.bash`:

    ```sh
    curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
    ```
2. Add the following lines to `.bash_profile`:

    ```sh
    if [ -f ~/.git-completion.bash ]; then
        source ~/.git-completion.bash
    else
        echo "ERROR: First run 'curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash'"
    fi
    ```

## configuration

### Create SSH keys

Test your SSH connection:

```sh
ssh -T git@github.com
```

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

## Update GitHub fork repository

```sh
# Add the remote, call it "upstream":

git remote add upstream https://github.com/whoever/whatever.git

# Fetch all the branches of that remote into remote-tracking branches,
# such as upstream/master:

git fetch upstream

# Make sure that you're on your master branch:

git checkout master

# Rewrite your master branch so that any commits of yours that
# aren't already in upstream/master are replayed on top of that
# other branch:

git rebase upstream/master
```

If you don't want to rewrite the history of your master branch, (for example because other people may have cloned it) then you should replace the last command with `git merge upstream/master`. However, for making further pull requests that are as clean as possible, it's probably better to rebase.

If you've rebased your branch onto upstream/master you may need to force the push in order to push it to your own forked repository on GitHub. You'd do that with:

```sh
git push -f origin master
```

You only need to use the `-f` the first time after you've rebased.

Reference: https://stackoverflow.com/a/7244456

Note: A quick note that rather than having to rebase your own master branch to ensure you are starting with clean state, you should probably work on a separate branch and make a pull request from that. This keeps your master clean for any future merges and it stops you from having to rewrite history with `-f` which messes up everyone that could have cloned your version.

