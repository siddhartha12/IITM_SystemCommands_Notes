# L3.1 - Executing multiple commands
* command1; command2; command3;
	* Each command will be executed one after other
* command1 && command2
	* 2 will execute if 1 executes successfully
* command1 || command2
	* 2 will execute if 1 is not executed and vice versa

If you use parentheses to execute commands, it is executed in another bash subshell before it returns. For each set of parentheses, the bash subshell moves one level down.

## File descriptors
has 3 parts
* stdin
* stdout
* stderr

## Commands
```
command > file1
# this will create a file1 if it doesn't exist and then write the output of the command to it, if it exists, it will overwrite

cat > file1
# here file 1 will be written into whatever you type into the terminal, to exit we use ctrl D

command >> file1
# appends to the file
```

# L3.2 - Redirections
we use the 2> command to keep the command line clean
```
ls $HOME .blah 2> error.txt

# The 2> command redirects all the stderr into a specified file, we can split our stdout and stderr outputs into different files

command > file1 2> file2

# Also this command won't return anoything on the command line. 
# all the outputs are redirected into the files specified

command > file1 2> &1
# This will write both the outputs into the same file
```


## going from files to command line
```
command < file1
# will take the contents from the file and execute it into the command
```


## Multiple commands
```
command1 | command 2
```
![[Pasted image 20251025131351.png]]

## /dev/null
is a kind of a sink of the system, anything sent here will just be gone like a blackhole
```
# can use
command > file1 2> /dev/null
```

## tee
writes to both files and command line
```
ls $HOME | tee file1
# this output of it will be the same as cat file1

ls $HOME | tee file1 file2
# this will write the succesful output to both the files same stuff, if we put an error command before tee also, only the success part is written

ls $HOME /blah | tee file1 file2 | wc -l
# this outputs the lengths of the files
# here hte wc outputs from the tee, not from the files

ls $HOME /blah 2> dev/null | tee file1 file2 | wc -l
# here we won't see an error output
```

# L3.1 - Software Managment
Using a package manager
## Need for package manager
* tools for installing, updating, removing and manging software
* Install new/updated software across network
* Package - File look up, both ways
* Database of packages on the system including versions
* Dependency checking
* Signature verification tools
* tools for building packages
![[Pasted image 20251025133005.png]]

![[Pasted image 20251025133018.png]]
![[Pasted image 20251025133034.png]]

## Inquiring package db
* search packages for a keyword
	* apt-cache search *keyword*
* List all packages
	* apt-cache pkgnames *keyword*
* displa package records of a package
	* apt-cache show -a package
```
apt-cahce pkgnames | less
# this will return a list according to storage
# we can make it sorted using

apt-cache pkgnames | sort | less

# to show packages with beginning letters
apt-cache pkgnames keyword
```

![[Pasted image 20251025133711.png]]

## Package priorities
* required: essential to system functioning
* important: provides functionality that enables system to run well
* standards: included in standard installation
* optional: can omit if no storage
* extra" could conflict with packages with higher priority, has specialized requirements, install if neeeded
```
apt-cache show function_name
```

## package sections
https://packages.ubuntu.com/focal

## checksums
way to verify the authenticity of the file
* md5 - 128bit
* sha1 - 160bit
* sha256 - 256bit

# L3.4 - Software Mangement p2
Only sudoers can install/upgrade/remove packages
/etc/sudoers

su - super
do - perform the action
```
# we can execute 
sudo cat /etc/sudoers
# to get a list of all the users who are sudoers
# if you are not a sudoer, then access will be denied and a log will be written that someone was trying to access illegally
# if you are accessing from a sudoers account, we can see the users

# to get the failed illegal attempt we can access the file /var/log/auth.log
# we can try and access it
tail -n 100 auth.log
# to get the last 100 lines
# but will be denied, we need to evoke superuser

sudo tail -n 100 auth.log
```

When istallling apackage, the system knows where to download the update the packages
the file is located at /etc/apt
files: source.list
folder: sources.list.d

## Updating
```
sudo apt-get update

# to install
sudo apt-get install module

# update a file
sudo apt-get reinstall module
```

## Installing/Updating
* Synchronize package overview files
	* apt-get update
* Upgrade all installed packages
	* apt-get upgrade
* install a package
	* apt-get install package
* reinstall
	* apt-get reinstall package

## Removing/Cleaning up
* Remove packages that were automatically installed to satisfy a dependency and not needed
	* apt-get autoremove
* Clean local repository of retreived package files
	* apt-get clean
* remove a package
	* apt-get remove package
* Purge package files from the system
	* apt-get purge package


## package mangement
* Package management in ubuntu using /var/lib/dpkg
located at /var/lib/dpkg

Files: arch, available, status
Folder: info


## Using dpkg
* list all packages whose names match the pattern
	* dpks -l pattern
* list installed files that come from packages
	* dpkg -L package
* Report the status of packages
	* dpkg -s package
* Search installed packages for a file
	* dpkg -S pattern
```
# to output the dpkg-query in a certain format
dpkg-qury -w -f='${section} ${binary:package}\n'

# can get this in a less page and sorted using
dpkg-qury -w -f='${section} ${binary:package}\n' | sort | less 

# if we want to see results that only contain a certain keyword, we can use grep
dpkg-qury -w -f='${section} ${binary:package}\n' | grep shells
# the above will return only the sections that are of shells
```


# L3.5 - Linux Process Management
can use sleep command to put the command line to sleep
```
sleep 3
# puts to sleep for 3 seconds
# during which cannot enter anything into the command line

# we want it in the background, we can use coproc
coproc sleep 30
# happens in the bacground, can still run the command line
# this function outputs the process ID

# now if we check the backgrund processes using
ps --forest 
# we can see the processes and that sleep is a child of bash

# to kill a process
kill -9 <processid>

# instead of a coprocess, i.e. simultaneous process, we can put the process in the background
sleep 30 &
# just use an ampersand after the command

ps
# the above will show that alongside bash, sleep is running, not a child

fg
# to move it to foreground, this will disable the cmd here, need to wait for the function tto finish

jobs
# this command is used to see any of the processes in the background

top
# this command is basically task manager, can see what processes are occupying how much resources

ctrl+z
# this here would suspend the process but keep it in the background, not kill it, can revive it using fg again

q or ctrl+c
# kills the process
```


## Bash process management
shell capabilities can be limited or expanded based on the way it was launched in
```
echo $-
# this outputs the capabilities of the current bash shell, and can be referenced against man page of the bash

# let's say we can launch another bash with less capabilities enabled
bash -c "echo \$-"
# the above command launches a bash subshell or child shell within that has less capabilities
# the child shell simply existed for as long as the command was executed

bash -c "echo \$-; ps --forest"
# can see that ps is a child of the child bash
```
Reference: (cuz it's all confusing)
```
echo \$-
# the above will print the permissions

echo \$$
# this will print the process id
```

## History
history command will give past commands with id, can run that command using
```
history

!<command_id>
```

## Brace Expansion
```
echo {a..z}
# the .. acts saying to fill the range
# can use in multiple also

echo {a..z}{D..K}
# this will return a cross product of the sets

echo *
# this will give the directories in the current directory

echo D*
# this will return the directories starting with D

echo Do*
# starting wth Do

echo $?
# this will give the return code of the last command, (0 if success, any value between 1-255 if otherwise)
# 127 code is when file is not found, in that case, the computer offers a few alternatives
# 130 for killed process from ctrl+C
# 137 for processes killed using -9

```

## Bench Calculator
```
bc
# then can enter expressions for evaluation
```
