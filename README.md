# symlink2text
This program will iterate through a tree of files and will find all of the symlinks.  It will then replace these with a text file with the same name and permissions which records where the symlink formerly pointed.  The text file generated looks like this.

```
#symlink2text
#https://github.com/s-andrews/symlink2text
TARGET  normal.txt
```

The purpose of the script is for cases where you need to transfer a file structure which contains symlinks to a file system which does not support them (for example a synology NAS).  This will at least record the symlink structures in a way which preserves the information and can potentially be reversed at a later date.

## WARNING
**THIS SCRIPT WILL BREAK ALL SYMLINKS IN THE FOLDER YOU RUN IT ON**

This is by design - it replaces symlinks with text files, so although the information is preserved the links will **not** work in the usual way after the script has been run.

I would strongly recommend running the script with the ```--dryrun``` option initially to check what it's going to do, before actually allowing it to make changes to your system.

## Usage

```
usage: symlink2text [-h] [--quiet] [--dryrun] [--version] startdir

Convert symlinks to a text file saying where the symlink pointed

positional arguments:
  startdir    The location to search for symlinks

optional arguments:
  -h, --help  show this help message and exit
  --quiet     Suppress all progress messages
  --dryrun    Report on actions but don't do anything
  --version   show program's version number and exit
```

## Testing

The program comes with a test file structure which you can run it on to check that it's working on your system.  The folder to test on is the ```testdata``` folder.  There are symlinks in there which should be modified.  Some of the symlinks point to the ```nontestdata``` folder, and files in that folder should **not** be modified.

```
$ ./symlink2text --dryrun testdata
WARNING: This script should be run with root level permissions. You may not be able to replace some symlinks
Found testdata/nontestdata which pointed to ../nontestdata with owner 13779 and group 14998
Found testdata/absolute.txt which pointed to /dev/null with owner 13779 and group 14998
Found testdata/relative.txt which pointed to normal.txt with owner 13779 and group 14998
Found testdata/nonexistant.txt which pointed to /i/dont/exist with owner 13779 and group 14998
Found testdata/folder which pointed to Subfolder with owner 13779 and group 14998
Found testdata/Subfolder/infolder.txt which pointed to ../normal.txt with owner 13779 and group 14998
```

## Permissions
Since this script will be iterating through a whole tree of files then you may need to replace files which are not owned by you.  In these cases you will need to run this script with root level privileges to allow access to these files.  If you are only modifying files owned by you then you do not need root permissions, but you will get a warning when the script is first launched.