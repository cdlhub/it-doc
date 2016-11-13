
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

## sed command

**Reference:** [LFCS: How to use GNU ‘sed’ Command to Create, Edit, and Manipulate files in Linux – Part 1](http://www.tecmint.com/sed-command-to-create-edit-and-manipulate-files-in-linux/) by TecMint.

## References

- [BEGINNER’S GUIDE FOR LINUX – Start Learning Linux in Minutes](http://www.tecmint.com/free-online-linux-learning-guide-for-beginners/) by TecMint, this course module is specially designed and compiled for those beginners, who want to make their way into Linux learning process and do the best in today’s IT organizations. This courseware is created as per requirements of industrial environment with complete entrance to Linux, which will help you to build a great success in Linux.
