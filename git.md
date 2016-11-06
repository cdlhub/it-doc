# Git

## configuration

### Create SSH keys (Linux, MacOS X)

1. Create a pair of SSH key.

    ```sh
    $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    Generating public/private rsa key pair.
    ```
2. Name your key based on its purpose (including `~/.ssh/` folder) and then enter a strong passphrase.

    ```sh
    Enter file in which to save the key (/home/you/.ssh/id_rsa):  [~/.ssh/<id-github-user>_rsa]
    ```
4. Configure `ssh-agent` to remember your passphrase during each session time. Add the following snippet at the end of `.bashrc`.

    ```sh

    # Automatically start ssh-agent and load the ssh-key(s) on login:
    if [ -z "$SSH_AUTH_SOCK" ] ; then
      eval `ssh-agent -s`
      ssh-add
    fi
    ```
5. Add github.com to SSH known host.

    ```sh
    you@EQECAT-LINUX:~$ ssh-keyscan -t rsa github.com > ~/.ssh/known_hosts
    ```


## SSH configuration for multiple GitHub account

**Reference:** Adapted from [Multiple SSH Keys settings for different github account](https://gist.github.com/jexchan/2351996).

Create one SSH key per GitHub account. You cannot use the same key for two different GitHub accounts.

```sh
# Go to your SSH configuration folder.
cd ~/.ssh

# Run the following command for each GitHub account.
# Name your pair of key files with your GitHub user name: '<github-user>_rsa'.
ssh-keygen -t rsa -b 4096 -C "your_email_1@youremail.com"

# Add your key to SSH agent
ssh-add ~/.ssh/<github-user>_rsa
# Under MacOS X you can even add it to your Keychain
# ssh-add -K ~/.ssh/<github-user>_rsa

# Add github.com to SSH known host.
ssh-keyscan -t rsa github.com > ~/.ssh/known_hosts
```

Please refer to github ssh issues for common problems.

for example, 2 keys created at:

~/.ssh/id_rsa_activehacker
~/.ssh/id_rsa_jexchan

then, add these two keys as following

$ ssh-add ~/.ssh/id_rsa_activehacker
$ ssh-add ~/.ssh/id_rsa_jexchan

you can delete all cached keys before

$ ssh-add -D

finally, you can check your saved keys

$ ssh-add -l

Modify the ssh config

$ cd ~/.ssh/
$ touch config
$ subl -a config

Then added

#activehacker account
Host github.com-activehacker
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_activehacker

#jexchan account
Host github.com-jexchan
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_jexchan

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
