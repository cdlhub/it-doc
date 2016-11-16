
## Directory Structure

**See** [Linux Directory Structure and Important Files Paths Explained](http://www.tecmint.com/linux-directory-structure-and-important-files-paths-explained/)

## pwd command

- `pwd` -- Print Working Directory and follow symbolic link.
- `pwd -L` -- Print working directory and do not follow symbolic link (print the name of the link).

## pushd/popd (cd) command

- `cd -` -- Move to the last visited folder.
- `pushd path/to/dir` -- Save the current directory in a stack and move to `path/to/dir`.
- `popd` -- Move to the last saved path with `pushd`.


## ls command

**Reference:** [15 Basic ‘ls’ Command Examples in Linux](http://www.tecmint.com/15-basic-ls-command-examples-in-linux/).

- `ls -lh` -- Display folder size in human readable format.
- `ls -F` -- Display trailing slash `/` of folder.
- `ls -R` -- List sub-folder content recursively.
- `ls -t` -- Sort files by latest modification first.
- `ls -S` -- Sort by file size.

## touch command (create empty file)

- `touch <file-name>`
- `touch <f1> <f2> ... <fn>` -- Create multiple files.
- `touch -a <file-name>` -- Set last access time to the current date and time.
- `touch -m <file-name>` -- Set modification time to the current date and time.
- `touch -t [[CC]YY]MMDDhhmm[.SS] <file-name>` -- Set last access and modification date to the specified date and time (`-c` do not create file if it does not exist).
- `touch -r <source-file> <destination-file>` -- Update timestamp of `<destination-file>` timestamp of `<source-file>`.

Use `-c` option if you don't want to create the file if it does not exist.

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

**Reference:** [LFCS: How to use GNU ‘sed’ Command to Create, Edit, and Manipulate files in Linux – Part 1](http://www.tecmint.com/sed-command-to-create-edit-and-manipulate-files-in-linux/) by TecMint.

## Bash

### Modify a string with regexp

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

## References

- [BEGINNER’S GUIDE FOR LINUX – Start Learning Linux in Minutes](http://www.tecmint.com/free-online-linux-learning-guide-for-beginners/) by TecMint, this course module is specially designed and compiled for those beginners, who want to make their way into Linux learning process and do the best in today’s IT organizations. This courseware is created as per requirements of industrial environment with complete entrance to Linux, which will help you to build a great success in Linux.
