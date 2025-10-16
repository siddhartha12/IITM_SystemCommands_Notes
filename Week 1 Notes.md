# L1.1 - Launching a Linux VM

Installed an Ubuntu 24 through VirtualBox 

# L1.2 - Command Line Environment

* ls - lists all files
	* -a = all files hidden
	* -l = lists the full names of the files (discussed later)
* ps - shell properties
* pwd - return current location
* uname - Operating System name
* clear or Ctrl+L- clear the screen
* exit or Ctrl+D - exit the terminal
* man - manual page (man __argument__)
	* man --pg.num-- --argument--

![[Pasted image 20251015230834.png]]

## File system
![[Pasted image 20251015231405.png]]

### traversing the tree
* / is the root of the file system
* / also the delimiter for sub directories
* . current directory
* .. parent directory
Path can be absolute or relative

* cd change directory
	* cd - goes back to default directory
	* cd .. - parent directory

## Folders Meaning
/bin - Essential Command binaries
/boot - static files of the boot loader
/dev - device files
/etc - host specific system configuration
/lib - essential shared libraries and kernel modules
/media - mount points for removable devices
/mnt - mount points
/opt - add on application software packages
/run - data relevant to rnning processes
/sbin - essential system binaries
/srv - data for services
/tmp - temporary files
/usr - secondary hierarchy
/var - variable data

### usr hierarchy

![[Pasted image 20251016002829.png]]

## var hierarchy
![[Pasted image 20251016002859.png]]

![[Pasted image 20251016002910.png]]


# L1.3 - simple commands
* date - timestamp
	* -R = in rfc5322 standard
* cal - calendar
	* cal month year - will output calendar of that year
* free - gives memory metrics
	* -h: in gigabytes
* groups will give users

![[Pasted image 20251016004722.png]]

![[Pasted image 20251016010533.png]]


- - ->regular file, all types
- d -> directory
- l -> symbolic link
- b, c -> block files, character files, they could be hard disk and terminal respectively
- s -> socket file -> process happening behind the scenes, can listen to channels and communication etc. 2 way communicable they are if you know what you're doing
- p -> pipe, process behind with one way communication, only out, no in
## Permission string
7 - rwx
6 - rw-
5 - r-x
4 - r--
1 - --x
0 - ---

change permission
* mkdir - make directory
* chmod - file permission modify
	* u - for user
	* g - for groups
	* o - for others
	* +/- defines whether to add permission or remove
	* r, w, x -> read write execute
	```
	  # the below removes the write permission for the group users
	  chmod g-w <filename>
	  
	  # remove executable
	  chmod g-x <filename>
	  ```
can
can do the above for each permission but can define it numerically also
```
chmod 700 <filename>
```

## Touch, copy, move, remove
basically change the last modified of the file but if the file doesn't exist, it will create
```
touch file1

# to copy the file into another file
cp file1 file2
# copies contents of file1 into file2

# move
mv file ..
# moves contents into parent directory
# can also rename files with the above command
mv file1 FileOne

# for remove, we have 
rm <filenmae>
# but this will prompt for confirmation, to not prompt, we can use -i
# to make the -i default we can use alias
alias rm='rm -i' 
# after which every command you run will run rm -i
```
## inode
```
ls -i <name>
```
inodes relate to the next concept of hardlinks
## Hardlinks
first get the inode with long form for all files including hidden
```
ls -lia
```
so hardlinks are basically like relations between files and folders. inode basically refers to the identity of the location of a directory.
for eg:
```
<nodeno> d<permissions> <hardlinks> 

<hardlinks here basically shows how many directories or files are linked to it
. will give how many directories in the present
.. the directories in the parent folder
only 1 hardlink usually means it's a file that resides here and is the only copy of it
```

## Read - file, Less
```
less filename
# this will allow you to read the text file page by page and then exit by pressing q

file filename
# will show the type of file or folder it is, what the format is etc. works on directories also
```


# L1.4 - Simple commands in Linux 2

/ - as a leading term between directories, can use multiple bbut ends up having the same effect

root folder is it's own parent so even if you do cd ..  from there it will remain in the same folder

ls:
* short and long form options
* Interpretation of directory as an argument
* Recursive listing
* Order of options on command line
```
ls -l
# lists everything long

ls -l level1
# shows inside the folder, not about the folder itself

ls -ld level1
# shows info only about the folder
# also the options and arguments can be in any goddamn series that they want
# But the ideal form would be
<command> -<options> <argument>
```

## File commands
* less - it will open the file in a file mode, can move back and forth between pages and use q to exit
* cat - concatenate - basicallyy dumps the entire file in the command line
* head - by default shows first 10 lines
	* head -n 5 filename (shows first 5 lines)
* tail - same as head but from bottom
* wc - wc filename
	* outputs "lines characters bytes ocupyd"
* more - file perusal filter for crt viewing
* stat - gives the size and statistics of the file

## commands
* which - which command 
	* returns the location that the command is location
* whatis - whatis command
	* returns the purpose of the function
* man 
	* returns the manual page for that command
* apropos - search manualpage descriptions
* info - gives a list of commands
* help - shell builtin, basically starting point, lists out formats and standard for all the other helping commands
* type - gives the type of a function whether it is of shell or os or an alias
* who - who is logged into system


## Multiple Arguments
* second argment
* Interpretation of last argument
* Recursion assumed for mv and not cp

eg: remove a non empty directory
```
rmdir mydir
# not happen cuz non empty folder

rm -r mydir
# this triggers recursive removal
```
another: copy one directory into another directory
```
cp mydir mydir2
# does not work

cp -r mydir mydir2
# make it recursive
```
Some commands assume recursion some do not need to check which ones do

## Links
ln is the command used to make links, -s option used for symbolic link
```
# use this to create a symbolic or soft link
touch file1
ln -s file1 file2
```
for hard links
```
touch file1
ln file1 file3
```

With hard links the inode number will be the same, symbolic link is a different inode that is purely a link, while the hard link is itself a file

## Stat, du for file sizes
gives the size of the file and other details
```
stat znew
# gives size blocks etc.

du znew
# 8    znew

# make it human readable
du -h znew
# 8.0K znew
```
basically the summary here is that even if the file size is smaller than the block, the file will occupy the full block instead of just a part of a block. the part of the block is not allowed, a block for this context is 4096 bytes. if a program is smaller than that, it will still take up the full block. If any program is even 1 byte larger than that, the complete next block will be allocated to the file. it will end up taking 8Kb on disk

## In-memory filesystems
* /proc - for kernels before 2.6
* /sys -  for newer kernels
special file systems, can read them from memory, they are not located on disk
```
cd /proc
# this will take you to the ram
# can see all the processes and data there
# the data shows 0 for processes and storage
# There is also a theoretical file for Linux which shows 140TB but does not exist, it is simply a definition of maximum virtual ram

ls /proc
# this one lists all the folders. 
# each of the numerical folders refer to one of the processes that the ram is currently holding 
# can see all the devices and drivers and products etc.
```