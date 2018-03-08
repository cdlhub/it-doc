**Inventory file**: basically a list of grouped servers. They are setup in `/etc/ansible/hosts`:

```
[example]www.example.com

[local-dev]
192.168.xxx.xxx
```

Test your inventory file:

```sh
# Connect to all servers of group exemple and run ping
ansible example -m ping -u [username]

# Check disk usage on servers
ansible example -a "free -m" -u [username]
```

Ansible allows admins to run ad-hoc commands on one or hundreds of machines at the same time, using the ansible command.

Sample inventory file with multiple servers:

```
# Lines beginning with a # are comments, and are only included for# illustration. These comments are overkill for most inventory files.# Application servers[app]192.168.60.4192.168.60.5# Database server[db]192.168.60.6# Group 'multi' with all servers[multi:children]appdb# Variables that will be applied to all servers[multi:vars]ansible_ssh_user=vagrantansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key```

**Remark:** Ansible assumes you’re using passwordless (key-based) login for SSH

**Facts**: environment details for a server.

## Commands

```sh
# Run [command] on all (-a) host in [group]
ansible [group] -a [command]
# ansible -a [command] [group]

# Get an exhaustive list of all the "facts"
ansible [host-or-group] -m setup

# Display host names
ansible multi -a "hostname"
# Check disk space
ansible multi -a "df -h"
# Check memory
ansible multi -a "free -m"
# Manually check if dates are in sync
ansible multi -a "date"

# Sync date and time on servers
ansible multi -s -a "service ntpd stop"ansible multi -s -a "ntpdate -q 0.rhel.pool.ntp.org"ansible multi -s -a "service ntpd start"
```

Options:

- `-f n` (i.e. `-f 1`) run command on `n` forks. Ansible commands are parallel by default.
- `-s` run command with `sudo` (`-K` to ask sudo password).
- `-m` use ansible module instead of shell command.
- `--limit` limit the command to a specific host in the specified group. It will match either an exact string or a regular expression (prefixed with ∼). Try to reserve the `--limit` option for running commands on single servers. If you often find yourself running commands on the same set of servers using `--limit`, consider instead adding them to a group in your inventory file.

	```sh
	# Example: apply the command to only the .4 server
	# Limit hosts with a simple pattern (asterisk is a wildcard).	ansible app -s -a "service ntpd restart" --limit "*.4"
		# Limit hosts with a regular expression (prefix with a tilde).	ansible app -s -a "service ntpd restart" --limit ~".*\.4"
	```
- Run operations in the background (tell Ansible to run the commands asynchronously, and poll the servers to see when the commands finish). While a background task is running, you can also check on the status elsewhere using Ansible’s `async_status` module, as long as you have the `ansible_job_id` value to pass in as `jid` (`ansible multi -s -m async_status -a "jid=763350539037"`):	- `-B <seconds>` the maximum amount of time (in seconds) to let the job run.
	- `-P <seconds>` the amount of time (in seconds) to wait between polling the servers for anupdated job status. Set `-P` to 0, so Ansible fires off the command then forgets about it. Running the command this way doesn’t allow status updates via `async_status` and a `jid`, but you can still inspect the file `∼/.ansible_async/<jid>` on the remote server.

## Modules

When you use Ansible’s modules instead of plain shell commands, you can use the powers of abstraction and idempotency offered by Ansible.

- `yum` and `apt`.
- `service` 
- `shell` to wrap command in ansible module.
- `group` and `user` to manage system groups and users (see [user module](https://docs.ansible.com/ansible/user_module.html) for parameters like `password`). You can do just about anything you could do with useradd, userdel, and usermod using Ansible’s user module.
- `stat` to check a file’s permissions, MD5, or owner (e.g. `ansible multi -m stat -a "path=/etc/environment"`).
- `copy` to copy a file to the servers (e.g. `ansible multi -m copy -a "src=/etc/hosts dest=/tmp/hosts"`). The `src` can be a file or a directory. If you include a trailing slash, only the contents of the directory will be copied into the `dest`. If you omit the trailing slash, the contents and the directory itself will be copied into the `dest`. The `copy` module is perfect for single-file copies, and works very well with small directories. When you want to copy hundreds of files, especially in very deeply-nested directory structures, you should consider either copying then expanding an archive of the files with Ansible’s `unarchive` module, or using Ansible’s `synchronize` module.
- `fetch` to retrieve a file from the servers (e.g. `ansible multi -s -m fetch -a "src=/etc/hosts dest=/tmp"`). Fetch will, by default, put the `src` from each server into a folder in the destination with the name of the host. You can add the parameter `flat=yes`, and set the dest to `dest=/tmp/` (add a trailing slash), to make Ansible fetch the files directly into the `/tmp` directory. Only use `flat=yes` if you’re copying files from a single host.
- `pip` for python packages.
- `mysql_*`
- `cron` setup cron tasks.

### Shell command

Why use shell instead of command? Ansible’s command module is the preferred option for running commands on a host (when an Ansible module won’t suffice), and it works in most scenarios. However, command doesn’t run the command via the remote shell `/bin/sh`, so options like `<`, `>`, `|`, and `&`, and local environment variables like `$HOME` won’t work. shell allows you to pipe command output to other commands, access the local environment, etc.

It’s best to use an Ansible module for every task. If you have to resort to a regular commandline command, try the the `command` module first. If you require the options mentioned above, use `shell`.

### Configure MySQL

Use modules `mysql_*`.

MySQL installs a database named `test` by default, and it is recommended that you remove the database as part of MySQL’s included `mysql_secure_installation` tool:

```yaml
- name: Remove the MySQL test database.      mysql_db: db=test state=absent
```

Create a database:

```yaml
- name: Create a database for Drupal.      mysql_db: "db={{ domain }} state=present"
```

In MySQL’s case, Ansible uses the MySQLdb Python package (python-mysqldb) to manage a connection to the database server, and assumes the default root account credentials (‘root’ as the username with no password). Obviously, leaving this default would be a bad idea! On a production server, one of the first steps should be to change the root account password, limit the root account to localhost, and delete any nonessential database users.

If you use different credentials, you can add a .my.cnf file to your remote user’s home directory containing the database credentials to allow Ansible to connect to the MySQL database without leaving passwords in your Ansible playbooks or variable files. Otherwise, you can prompt the user running the Ansible playbook for a MySQL username and password. This option, using prompts, will be discussed later in the book.

### Create directories and files
You can use the file module to create files and directories (like `touch`), manage permissions and ownership on files and directories, modify SELinux properties, and create symlinks.
```sh# Create a directory
ansible multi -m file -a "dest=/tmp/test mode=644 state=directory"

# Create a symlink (set state=link)ansible multi -m file -a "src=/src/symlink dest=/dest/symlink owner=root group=root state=link"
```

### Delete directories and filesYou can set the state to absent to delete a file or directory.```shansible multi -m file -a "dest=/tmp/test state=absent"
```Also read the documentation for the other file-management modules like `lineinfile`, `ini_file`, and `unarchive`.

### Deploy a version-controlled application```sh
# First, update the git checkout to the application’s new version branch, 1.2.4, on all the app servers:ansible app -s -m git -a "repo=git://example.com/path/to/repo.git dest=/opt/myapp update=yes version=1.2.4"

# run the application’s update.sh shell script:ansible app -s -a "/opt/myapp/update.sh"
```## Playbooks

Ansible keeps track of the state of everything on all our servers. If you run the playbook the first time, it will provision the server. Even better, the second time you run it (if the server is in the correct state), it won’t actually do anything besides tell you nothing has changed. Additionally, running the playbook with the `--check` option verifies the configuration matches what’s defined in the playbook, without actually running the tasks on the server.