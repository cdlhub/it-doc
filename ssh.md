# SSH

**Content**

<!-- TOC depthFrom:2 -->

- [Permanently Store SSH Key to macOS Keychain](#permanently-store-ssh-key-to-macos-keychain)
- [Copy Public Key to Server](#copy-public-key-to-server)
- [Check SSH Password](#check-ssh-password)

<!-- /TOC -->

## Permanently Store SSH Key to macOS Keychain

Reference: [How can I permanently add my SSH private key to Keychain so it is automatically available to ssh?](https://apple.stackexchange.com/questions/48502/how-can-i-permanently-add-my-ssh-private-key-to-keychain-so-it-is-automatically/250572#250572)

1. Store key in the keychain (run it only once):

    ```sh
    ssh-add -K ~/.ssh/<private-key>
    ```
2. Update `~/.ssh/config` with the following lines:

    ```sh
    Host *
        UseKeychain yes
        AddKeysToAgent yes
        IdentityFile ~/.ssh/id_rsa
        IdentityFile ~/.ssh/<private-key>
        ...
    ```
    You can add as many `IdentityFile` lines as you have keys.

## Copy Public Key to Server

Reference: [SSH-COPY-ID](https://www.ssh.com/ssh/copy-id).

```sh
ssh-copy-id -i <id_rsa_private_file> [user@]<hostname>
```

Common options:


```sh
ssh-copy-id [-f] [-n] [-i identity file] [-p port] [-o ssh_option] [user@]hostname
```

- `-i` -- Specifies the identity file that is to be copied (default is `~/.ssh/id_rsa`). If this option is not provided, this adds all keys listed by `ssh-add -L`. **Note**: it can be multiple keys and adding extra authorized keys can easily happen accidentally! If `ssh-add -L` returns no keys, then the most recently modified key matching `~/.ssh/id*.pub`, excluding those matching `~/.ssh/*-cert.pub`, will be used.
- `-p` -- port Connect to the specifed SSH port on the server, instead of the default port 22.
- `-n` -- Just print the key(s) that would be installed, without actually installing them.
- `-f` -- Don't check if the key is already configured as an authorized key on the server. Just add it. This can result in multiple copies of the key in authorized_keys files.
- `-o ssh_option` -- Pass `-o ssh_option` to the SSH client when making the connection. This can be used for overriding configuration settings for the client. See ssh command line options and the possible configuration options in `ssh_config`.
- `-h` or `-?` -- Print usage summary.

## Check SSH Password

Reference: [How do I verify/check/test/validate my SSH password?](https://stackoverflow.com/questions/4411457/how-do-i-verify-check-test-validate-my-ssh-password#23666831)

`ssh-keygen -y` will prompt for the passphrase if there is one.

```sh
ssh-keygen -y -f <private-file>
Enter passphrase:
```

Results:
- Correct passphrase: display public key.
- Wrong passphrase: display `load failed`.
- If the key has no passphrase, it will not prompt you for a passphrase and will immediately show you the associated public key.

## Add Key to SSH Agent

### Windows

Using Git Bash, add the following lines to `.bashrc` (don't forget to append private key file to `ssh-add` command):

```sh
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add <private/id/file/path>
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add <private/id/file/path>
fi

unset env
```