<!-- TOC -->

- [man -- Manual pages](#man----manual-pages)
- [Time and date](#time-and-date)
    - [cal -- Display calendar](#cal----display-calendar)
    - [date -- Display date and time](#date----display-date-and-time)
- [Terminal](#terminal)
    - [tty -- Display terminal name](#tty----display-terminal-name)
    - [Detach process from current terminal](#detach-process-from-current-terminal)
    - [screen](#screen)
- [System identification](#system-identification)
    - [uname -- Print operating system name](#uname----print-operating-system-name)
- [Users and Groups](#users-and-groups)
    - [id -- List user identity and groups](#id----list-user-identity-and-groups)
    - [who -- List connected users](#who----list-connected-users)
    - [MacOS](#macos)
- [Partition](#partition)
- [Directory Structure](#directory-structure)
    - [Difference Between /opt and /usr/local](#difference-between-opt-and-usrlocal)
- [Environment variables](#environment-variables)
- [pwd command](#pwd-command)
- [pushd/popd (cd) command](#pushdpopd-cd-command)
- [realpath command](#realpath-command)
- [ls command](#ls-command)
- [tail command](#tail-command)
- [touch command (create empty file)](#touch-command-create-empty-file)
- [cat command](#cat-command)
- [Find disk usage (df)](#find-disk-usage-df)
- [Display file and folder size](#display-file-and-folder-size)
- [tar command](#tar-command)
    - [tar archive](#tar-archive)
    - [Untar archive](#untar-archive)
    - [List archive content](#list-archive-content)
    - [Tar Usage and Options](#tar-usage-and-options)
- [curl](#curl)
- [Display progress bar](#display-progress-bar)
- [sed command](#sed-command)
- [Bash](#bash)
    - [Special variables](#special-variables)
    - [Modify a string with regexp](#modify-a-string-with-regexp)
    - [Test if a command succeeded](#test-if-a-command-succeeded)
- [Find files](#find-files)
- [Search for installed libraries](#search-for-installed-libraries)
- [Search for linked library](#search-for-linked-library)
- [LD_LIBRARY_PATH environement](#ld_library_path-environement)
- [PatchELF](#patchelf)
- [Jobs](#jobs)
- [Process](#process)
- [rsync](#rsync)
- [Common systems administrator tasks](#common-systems-administrator-tasks)
    - [Security Configuration](#security-configuration)
    - [Database](#database)
- [MacOS X](#macos-x)
- [References](#references)

<!-- /TOC -->
## man -- Manual pages

```sh
# Search keyword for a command in manual
man -k <keyword>

# Select a section of manual pages
man -s <n> <command>
```

## Time and date

### cal -- Display calendar

```sh
# Current month
cal

# Specific month
cal <mm> <yyyy>

# Full year calendar
cal <yyyy>
```

### date -- Display date and time

```sh
date

# Format display ('+' is mandatory)
date "+DATE: %Y-%m-%d%nTIME: %H:%M:%S"
DATE: 2017-01-19
TIME: 17:56:06
```

## Terminal

### tty -- Display terminal name

```sh
tty
```

### Detach process from current terminal

Using the Job Control of bash to send the process into the background:

    - `Ctrl+Z` to stop (pause) the program and get back to the shell.
    - `bg` to run it in the background.
    - `disown -h [job-spec]` where `[job-spec]` is the job number (like `%1` for the first running job; find about your number with the `jobs` command) so that the job isn't killed when the terminal closes.

- `nohup`: http://linux.101hacks.com/unix/nohup-command/

### screen

```sh
# start screen
screen
```

- `Ctrl-a ?` -- Display help.
- `Ctrl-a c` -- Create a new _screen_ window.
- `Ctrl-a n` -- Switch to next window.
- `Ctrl-a d` -- Detach current _screen_ and go back to shell.
- `Ctrl-a H` -- Creates a running log of the session.

```sh
# List screens
screen -ls

# Detach screen
screen -D <id>

# Reattach screen or list screens
screen -r

# Reattach screen
screen -r <id>
```

- Type `exit` to stop current screen.

## System identification

### uname -- Print operating system name

```sh
# Print all information
uname -a

# Print architecture and machine hardware
uname -pm

# Print Hostname (same as 'hostname' command)
uname -n
```

## Users and Groups

- `/etc/passwd` -- Contains all users.
- `/etc/group` -- Contains all groups.

`passwd` is used to change password.

### id -- List user identity and groups

If `<user>` is ommited, then current user is used.

```sh
# Display user name
id -un <user>

# Display user full name
id -F <user>

# Display user identification as '/etc/passwd' entry
id -P <user>

# Display user name and groups
id -p <user>

# Display user groups
id -Gn <user>
```

### who -- List connected users

```sh
# List connected users and their terminal name
who

# List all info with header
who -aH
```

### MacOS

- `staff` -- Default group for all users.
- `admin` -- Group of all users with admin privilege.
- `wheel` -- Group for `root`. `wheel` should only have one user (`root`).

## Partition

- `lsblk` -- Tree-like list of all partitions and mount points
- `sudo lsblk -o col[,col]` where available `col` is:
	
	```
	    NAME  device name
      KNAME  internal kernel device name
    MAJ:MIN  major:minor device number
     FSTYPE  filesystem type
 MOUNTPOINT  where the device is mounted
      LABEL  filesystem LABEL
       UUID  filesystem UUID
         RO  read-only device
         RM  removable device
      MODEL  device identifier
       SIZE  size of the device
      STATE  state of the device
      OWNER  user name
      GROUP  group name
       MODE  device node permissions
  ALIGNMENT  alignment offset
     MIN-IO  minimum I/O size
     OPT-IO  optimal I/O size
    PHY-SEC  physical sector size
    LOG-SEC  logical sector size
       ROTA  rotational device
      SCHED  I/O scheduler name
    RQ-SIZE  request queue size
       TYPE  device type
   DISC-ALN  discard alignment offset
  DISC-GRAN  discard granularity
   DISC-MAX  discard max bytes
  DISC-ZERO  discard zeroes data
  ```
- `mkfs.<type> /dev/<part>` -- Format partition with the specified type. Use `-f` for fast format.
- `sudo blkid /dev/<part>` -- Display UUID of specified partition. Useful to setup `fstab`.
- `sudo mount -t <type> -o <options> /dev/<part> </mount/point/dir>`.
- `sudo mount </mount/point/dir>` -- Mount a device already in `/etc/fstab`.
- `sudo umount </mount/point/dir>` -- Unmount a device.

`parted` can be used to manage partition instead of `fdisk`:

```sh
sudo parted /dev/<device>
```

See [Introduction to fstab](https://help.ubuntu.com/community/Fstab) on `/etc/ƒstab` setup and options.

`fmask` and `dmask` mount options are used to define permissions (`umask` sets them to both files and directories, while `fmask` only applies to files and `dmask` to directories).

The masks are NOT the permissions of the file, they are used to get the permissions you want. In addition, masks can't add any permissions, they only limit what permissions a file or a directory can have.

Table to help understand how the masks permissions work :

```
    0   1   2   3   4   5   6   7
r   +   +   +   +   -   -   -   -
w   +   +   -   -   +   +   -   -
x   +   -   +   -   +   -   +   -
```

- The first character represents that its an octal permissions
- The second is for the owner
- The third is the group
- The fourth is for other, i.e any other user

Typical permissive option is: `fmask=0111,dmaks=0000`.

When `umount` fails, you can list used files with `sudo fuser -vm <mount/point>`.

Sample `fstab` record for NTFS file system:

```
UUID=02C461EBC461E201 /media/USBDVD1 ntfs rw,permissions,fmask=0111,dmaks=0000,auto,nosuid,nodev,allow_other 0 2
```

## Directory Structure

**See** [Linux Directory Structure and Important Files Paths Explained](http://www.tecmint.com/linux-directory-structure-and-important-files-paths-explained/)

### Difference Between /opt and /usr/local

While both are designed to contain files not belonging to the operating system, `/opt` and `/usr/local` are not intended to contain the same set of files.

`/usr/local` is a place to install files built by the administrator, typically by using the make command (e.g., `./configure; make; make install`). The idea is to avoid clashes with files that are part of the operating system, which would either be overwritten or overwrite the local ones otherwise (e.g., `/usr/bin/foo` is part of the OS while `/usr/local/bin/foo` is a local alternative).

All files under `/usr` are shareable between OS instances, although this is rarely done with Linux. This is a part where the FHS is slightly self-contradictory, as `/usr` is defined to be read-only, but `/usr/local/bin` needs to be read-write for local installation of software to succeed. The SVR4 file system standard, which was the FHS' main source of inspiration, is recommending to avoid `/usr/local` and use `/opt/local` instead to overcome this issue.

`/usr/local` is a legacy from the original BSD. At that time, the source code of `/usr/bin` OS commands were in `/usr/src/bin` and `/usr/src/usr.bin`, while the source of locally developed commands was in `/usr/local/src`, and their binaries in `/usr/local/bin`. There was no notion of packaging (outside tarballs).

On the other hand, `/opt` is a directory for installing unbundled packages, each one in its own subdirectory. They are already built whole packages provided by an independent third party software distributor. Unlike `/usr/local` stuff, these packages follow the directory conventions (or at least they should). For example, someapp would be installed in `/opt/someapp`, with one of its command being `/opt/someapp/bin/foo`, its configuration file would be in `/etc/opt/someapp/foo.conf`, and its log files in `/var/opt/someapp/logs/foo.access`.

Source: [What is the difference between /opt and /usr/local?](https://unix.stackexchange.com/questions/11544/what-is-the-difference-between-opt-and-usr-local/11552#11552)

## Environment variables

See [Environment Variables](https://help.ubuntu.com/community/EnvironmentVariables) Ubuntu doc. Contains list of common standard variables.

## pwd command

- `pwd` -- Print Working Directory.
- `pwd -P` -- Print Working Directory and follow symbolic link (Physical path).
- `pwd -L` -- Print working directory and print the name of the link. It does not follow symbolic link (Logical path).

## pushd/popd (cd) command

- `cd -` -- Move to the last visited folder.
- `pushd path/to/dir` -- Save the current directory in a stack and move to `path/to/dir`.
- `popd` -- Move to the last saved path with `pushd`.

## realpath command

Display absolute full path: `realpath <file-name>`.

## ls command

**Reference:** [15 Basic ‘ls’ Command Examples in Linux](http://www.tecmint.com/15-basic-ls-command-examples-in-linux/).

- `ls -lh` -- Display folder size in human readable format.
- `ls -F` -- Display trailing slash `/` of folder.
- `ls -t` -- Sort files by latest modification first.
- `ls -S` -- Sort by file size.
- `ls -d */` -- List directories only.
- `ls -d <name-with-wild-card>` -- List files without traversing directories recursively.
- `ls -R` -- List sub-folder content recursively.

List all symbolic links in a directory:

```sh
find . -type l -ls

# process the current directory
find . -maxdepth 1 -type l -ls
```

## tail command

- `tail -f <file-name>` -- "follow" the file prints last lines as they are written to the file.

## touch command (create empty file)

- `touch <file-name>`
- `touch <f1> <f2> ... <fn>` -- Create multiple files.
- `touch -a <file-name>` -- Set last access time to the current date and time.
- `touch -m <file-name>` -- Set modification time to the current date and time.
- `touch -t [[CC]YY]MMDDhhmm[.SS] <file-name>` -- Set last access and modification date to the specified date and time (`-c` do not create file if it does not exist).
- `touch -r <source-file> <destination-file>` -- Update timestamp of `<destination-file>` timestamp of `<source-file>`.

Use `-c` option if you don't want to create the file if it does not exist.

> 1. Use the command `touch ~/timestamp` to create and/or modify a timestamp file.
> 2. Perform actions...
> 3. Use the command `find -x / -newer ~/timestamp >~/changedfiles.txt` to generate a list of changed files.

## cat command

Concatenate multiple files:

```sh
# Concatenate multiple files to one file
cat <file1> <file2> <file3> > big_file

# Concatenate and sort content of files
cat <file1> <file2> <file3> | sort > sorted_big_file
```

## Find disk usage (df)

- `df -h` -- Human readable format.
- `df -T` -- Display type of volumes (Linux only, not MacOS X).
- `df -a` -- Display information of all file system disk.
- `df <folder>` -- Display info about the mounted volume the folder is in.

## Display file and folder size

- `du -h <folder>` -- Display folder and sub-folder size without individual files.
- `du -a <folder>` -- Include individual files.
- `du --max-depth=2` -- List subdirectories and their sizes to any desired level of depth.

Sort output by decreasing size:

```sh
du -hs * | sort -rh

# macOS
brew install coreutils
du -hs * | gsort -rh
```

## tar command

### tar archive

```sh
tar -cvf my-archive.tar path/to/my/folder
tar -czvf my-archive.tar.gz path/to/my/folder
```

- `c` – Creates a new .tar archive file.
- `v` – Verbosely show the .tar file progress.
- `f` – File name type of the archive file.
- `z` – Compress tar archive using gzip.

### Untar archive

- `x` – extract a archive file.

```sh
tar -xvf public_html-14-09-12.tar

# Untar files in specified Directory ##
tar -xvf public_html-14-09-12.tar -C /home/public_html/videos/

# Untar single file
tar -xvf cleanfiles.sh.tar cleanfiles.sh
tar -zxvf tecmintbackup.tar.gz tecmintbackup.xml

# Untar multiple files
tar -xvf tecmint-14-09-12.tar "file 1" "file 2"
tar -xvf Phpfiles-org.tar --wildcards '*.php'
```

### List archive content

```sh
tar -tvf uploadprogress.tar
tar -tvf uploadprogress.tar.gz
```

### Tar Usage and Options

- `c` – create a archive file.
- `x` – extract a archive file.
- `v` – show the progress of archive file.
- `f` – filename of archive file.
- `t` – viewing content of archive file.
- `j` – filter archive through bzip2.
- `z` – filter archive through gzip.
- `r` – append or update files or directories to existing archive file.
- `W` – Verify a archive file.
- `--wildcards` – Specify patters in unix tar command.
- `--exclude='PATTERN'`: exclude those files and directory which respect the pattern (using `*` and `?`)


## curl

`curl` transfers a URL. `curl` can be useful for determining if your application can reach another service, such as a database, or **checking if your service is healthy**.

- `-I` -- shows the header information.
- `-s` -- silences the response body.

Check what is the response when trying to reach my application:

```sh
curl -I -s myapplication:5000
HTTP/1.0 500 INTERNAL SERVER ERROR
```

## Display progress bar

**Reference:** [How to Monitor Progress of (Copy/Backup/Compress) Data using `pv` Command](http://www.tecmint.com/monitor-copy-backup-tar-progress-in-linux-using-pv-command/).

`pv` is used with other programs which lack the ability to monitor the progress of a an ongoing operation. You can use it, by placing it in a pipeline between two processes, with the appropriate options available.

The syntax of `pv` command as follows:

```sh
pv file
pv options file
pv file > filename.out
pv options | command > filename.out
comand1 | pv | command2
```

- Copy a file: `pv source.file > destination.file`
- Zip a file: `pv source.file | gzip > source.file.gz`

## sed command

## Bash

### Special variables

- `$1`, `$2`, `$3`, ... are the positional parameters.
- `$@` is an array-like construct of all positional parameters, `{$1, $2, $3 ...}`. The positional parameters are temporarily replaced when a shell function is executed.
- `$*` is the IFS expansion of all positional parameters, `$1 $2 $3 ...`.
- `$#` is the number of positional parameters.
- `$-` current options set for the shell.
- `$$` pid of the current shell (not subshell).
- `$_` most recent parameter (or the abs path of the command to start the current shell immediately after startup).
- `$IFS` is the (input) field separator.
- `$?` is the most recent foreground pipeline exit status.
- `$!` is the PID of the most recent background command.
- `$0` is the name of the shell or shell script.

### Modify a string with regexp

**Reference:** [LFCS: How to use GNU ‘sed’ Command to Create, Edit, and Manipulate files in Linux – Part 1](http://www.tecmint.com/sed-command-to-create-edit-and-manipulate-files-in-linux/) by TecMint.

Use `sed` to modify a string in a variable and assign it to another:

```sh
# No spacea around '=' sign
# Replace 'search' by 'replace' in $my_string and assign the result to $my_variable.
my_variable=$(sed 's/search/replace/' <<<$my_string)
```

### Test if a command succeeded

A succesful command always returns `0`. Shell `if <command>` statement tests if the command succeeded, that is when it returns `0`.

```sh
if command; then
    echo "Success"
else
    echo "Failure"
fi
```

If you just want to test for success or failure, whithout any output on `stdout` or `stderr`, you need to redirect all command outputs to `/dev/null`:

```sh
# stdout is redirected to /dev/null
# stderr is redirected to stdin
if command 1>/dev/null 2>&1; then
```

## Find files

How to find text in files (see [How to find all files containing specific text on Linux?](https://stackoverflow.com/questions/16956810/how-to-find-all-files-containing-specific-text-on-linux)):

```sh
grep -rnw '/path/to/somewhere/' -e "pattern"
```
- `-r` or `-R` is recursive,
- `-n` is line number, and
- `-w` stands match the whole word.
- Along with these, `--exclude` or `--include` parameter could be used for efficient searching. It is also possible to exclude/include directories through `--exclude-dir` and `--include-dir` parameter.

```sh
# only search through the files which have .c or .h extensions
grep --include=\*.{c,h} -rnw '/path/to/somewhere/' -e "pattern"

# exclude searching all the files ending with .o extension
grep --exclude=*.o -rnw '/path/to/somewhere/' -e "pattern"

# use of --exclude-dir
grep --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"
```

## Search for installed libraries

Use tool for Dynamic Linker Run Time Bindings configuration.

```sh
# -p -- Print cache
ldconfig -p | grep <libname>
```

## Search for linked library

```sh
# List libraries <prog-name> is linked to 
ldd <prog-name>
```

See what other libraries a program or dynamically linked library depends upon:

```sh
# Cygwin
cygcheck <libxx.dll>

# Linux
ldd <libxx.so>

# Mac
otool -L libxx.dylib
```

## LD_LIBRARY_PATH environement

The `LD_LIBRARY_PATH` environment variable contains a colon separated list of paths that the linker uses to resolve library dependencies of ELF executables at run-time. These paths will be given priority over the standard library paths `/lib` and `/usr/lib`. The standard paths will still be searched, but only after the list of paths in `LD_LIBRARY_PATH` has been exhausted.

You can check if the linker can locate all the required libraries by running the `ldd` command:

```sh
ldd path/to/program
```

Reference: [How to correctly use `LD_LIBRARY_PATH`](http://wiredrevolution.com/system-administration/how-to-correctly-use-ld_library_path)

## PatchELF

[PatchELF](https://nixos.org/patchelf.html) is a small utility to modify the dynamic linker and `RPATH` of ELF executables.

## Jobs

- `jobs -l` -- List processes with ID.
- `jobs -n` -- List processes that have changed status.
- `jobs -s` -- Restrict output to stopped jobs.
- `fg <n>` -- Put job to foreground.
- `bg <n>` -- Put job to background.
- `Ctrl-Z` -- Stop the running job (still in the job list).
- `kill -9 %<n>` -- Kill runnung job `n`.

## Process

```sh
# List parent ID of process with <process-id>
ps -o ppid= -p <process-id>
```

## rsync

```sh
rsync [options] [user@ip:]source [user@ip:]destination
```

Usual options: `-avzh`

Source can be:

- a file or a list of files: `file-name.txt`
- content of a folder (all its files and sub-directories): `path/to/folder/` (don't forget trailing `/`)
- an entire folder (the folder in itself): `path/to/folder` (no trailing `/`)

Options:

- `-n`, `--dry-run`: show what would have been transferred
- `-a` : archive mode, archive mode allows copying files recursively and it also preserves symbolic links, file permissions, user & group ownerships and timestamps
- `-v` : verbose mode
- `-z` : compress file data
- `-h` : human-readable, output numbers in a human-readable format
- `-e ssh` : transfer over ssh protocol
- `--delete`: delete files that don’t exist on the sending side
- `--progress`: show the progress while transferring the data
- `--include 'PATTERN'`: include only those files and directory which respect the pattern (using `*` and `?`)
- `--exclude 'PATTERN'`: exclude those files and directory which respect the pattern (using `*` and `?`)
- `-r`: copies data recursively (but don’t preserve timestamps and permission while transferring data
- `-u, --update`: skip files that are newer on the receiver

Troubleshooting:

- `rsync: failed to set times on "/foo/bar": Operation not permitted (1)`:

	If `/foo/bar` is on NFS (or possibly some FUSE filesystem), that might be the problem. Either way, adding `-O` / `--omit-dir-times` to your command line will avoid it trying to set modification times on directories.

## Common systems administrator tasks

- Apply patches and updates via yum, apt, and other package managers.
- Check resource usage (disk space, memory, CPU, swap space, network). • Check log files.
- Manage system users and groups.
- Manage DNS settings, hosts files, etc.
- Copy files to and from servers.
- Deploy applications or run application maintenance.
- Reboot servers.
- Manage cron jobs.

### Security Configuration

There are nine basic measures to ensure servers are secure from unauthorized access or intercepted communications:

1. Use secure and encrypted communication.
2. Disable root login and use sudo.
3. Remove unused software, open only required ports. 4. Use the principle of least privilege.
5. Update the OS and installed software.
6. Use a properly-configured firewall.
7. Make sure log files are populated and rotated.
8. Monitor logins and block suspect IP addresses.
9. Use SELinux (Security-Enhanced Linux).

### Database

There are things you should do to secure a production MySQL server, including:

- removing the test database,
- adding a password for the root user account,
- restricting the IP addresses allowed to access port 3306 more closely,
- and some other minor cleanups.

## MacOS X

```sh
# Force to eject a volume
diskutil unmountDisk force /Volumes/DISK_NAME

# Create ISO image
hdiutil makehybrid -iso -joliet -o ./IMAGE_NAME.iso /Volumes/DISK_NAME

# Bypass application first start verification
xattr -d com.apple.quarantine /Applications/APP_NAME.app
```

## References

- [BEGINNER’S GUIDE FOR LINUX – Start Learning Linux in Minutes](http://www.tecmint.com/free-online-linux-learning-guide-for-beginners/) by TecMint, this course module is specially designed and compiled for those beginners, who want to make their way into Linux learning process and do the best in today’s IT organizations. This courseware is created as per requirements of industrial environment with complete entrance to Linux, which will help you to build a great success in Linux.
