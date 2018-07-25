Sudo
====

**Reference**: [Understanding sudoers(5) syntax](http://toroid.org/sudoers-syntax).


- Configuration file: `/etc/sudoers`. Never edit this file with a normal text editor! Always use the `sudo visudo` command instead, because `sudo` will not work if `/etc/sudoers` contains syntax errors!
- Change `visudo` editor:
    - Ubuntu: run `sudo update-alternatives --config editor`.
    
        Output:
        ```sh
        There are 4 choices for the alternative editor (providing /usr/bin/editor).

        Selection    Path                Priority   Status
        ------------------------------------------------------------
        * 0            /bin/nano            40        auto mode
        1            /bin/ed             -100       manual mode
        2            /bin/nano            40        manual mode
        3            /usr/bin/vim.basic   30        manual mode
        4            /usr/bin/vim.tiny    10        manual mode

        Press <enter> to keep the current choice[*], or type selection number:
        ```
    - CentOS: add `export EDITOR=<name of prefered editor>` to `~/.bashrc`.

- Syntax:

    ```
    %group HOST=(USER:GROUP) OPTION1: OPTION2: COMMAND

    %admin ALL=(ALL:ALL) NOPASSWD: SETENV:   ALL
    ```

    - `HOST`: Host address/name from which `sudo` command can occur. Can be `ALL`.
    - `USER`: The user the command can be run as using `sudo -u`. Can be `ALL`. If set to `root`, all `sudo` command will be run as `root`, which is like disabling `-u`.
    - `GROUP`: The group the command can be run as using `sudo -g`. Can be `ALL`.
    - `OPTION:`: The most important options are `NOPASSWD` (to not require a password) and `SETENV` (to allow the user to set environment variables for the command). Other available options include `NOEXEC`, `LOG_INPUT` and `LOG_OUTPUT`, and SELinux role and type specifications.
    - `COMMAND`: A list of comma separated commands. Can be `ALL`.

