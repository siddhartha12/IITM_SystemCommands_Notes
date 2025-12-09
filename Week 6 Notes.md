# L6.1 - Command Line Utilities
Utilities
* find - locating files and processing them
* tar, gzip - packaging collections of files
* make - conditional actions

## Find utility
```
find [pathnames] [conditions]
```
flags
* `-name` - *pattern* to match filenames
* `-type` - file type code
	* c -> character file
	* d -> directory
	* l -> symbolic link
* `-atime` - files accessed 
	* `+n` (more than n days ago)
	* `-n` less than n days ago 
* `-ctime` - file changed
	* `+n` more than n days ago
	* `-n` less than n days ago
* `-mtime` - file modifier
	* `+n` more than n days ago
	* `-n` less than n days ago
* `-regex` - regular expression for *pattern* of filenames
		  combine with `-regextype posix-basix, posix-egrep`
* `-exec` - command to run using {} as placeholder for filename
* `-print` - print the full path name of matching files
* `-size` - size of file
	* `+sizenumber` - larger than these size
	* `-sizenumber` - smaller than sizenumber


## File packaging
* Deep file hierarchies
* Large number of tiny files
* `tar` - collect a file hierarchy into a single file
* `gzip` - compress a file
Applications: backup, file sharing, reduce disc utilization

Possibilities:
* `tar, zip`
* `compress (ncompress), gzip (ncompress), bzip2 (bzip2), xz (xz-utils), 7z (p7zip-full)`
* Tarballs like `bundle.tgz` for package + compress
* Time  & memory required to shrink/expand versus size ratio
* portability
* Unique names using timestamp, process ID

## Make
```
make -f make.file
```

# L6.2 - Overview of Shell Scripts

Creating your own commands

## Software tools principles
* do one thing well
* process lines of text, not binary
* Use regular expressions
* Default to standard I/O
* don't be chatty
* Generate same output format accepted as input
* Let someone else do the hard part
* Detour to build specialized tools

Used with pipe to combine with another function

Scripts are of two types
* sourced
  `. scriptname
  `source scriptname
	* PID same as current shell, commands are executed one after other shell environment continues
	* Used to prepare environment
* executed
  `./scriptname`
	* Needs execution permission
	* New process gets created to run script
	* PID not same as the shell
	* commands executed after one another
	* New environment lost after return

## Script Location
* Use absolute path or relative path while executing the script
* Keep the script in folder listed in `$PATH`
* watch out for sequence of directories in `$PATH`

## Bash environment
Login shell:
* `/etc/profile`
* `~/.bash_profile`
* `~/.bash_login`
* `~/.profile`

Non-login shell
* `/etc/bash.bashrc`
* `~/.bashrc`

## Output from shell scripts
* echo
	* simple terminated with a newline if `-n` option not given
* printf
	* supports format specifiers like C

## Input to shell scripts
* read var
	* string read from command line stored in $var


## Shell script arguments
* $0 name of shell program
* $# number of arguments passed
* $1 of ${1} - first argument
* $* or $@ - all arguments at once
* `"$*"` - all arguments as a single string
* "$@" - all arguments are separate strings

## Command Substitution
```
var = `command`
var=$(command)
```

## for do loop
```
for var in list
do
	commands
done
```
* commands are executed once for each item in the list
* space is the field delimiters
* set IFS if required

## case statement
```
case var in 
pattern1)
	commands
	;;
pattern2)
	commands
	;;
esac
```

## If loop
```
if condition
then 
	commands
fi

# or
if condition; then
	commands
fi
```

## while do loop
```
while condition
do 
	command
done
```
commands are executed only if condition returns true

## until do loop
```
until condition
do
	commands
done
```

## functions
```
myfunc ()
{
	commands
}
```

## Conditions
* test
	* `test -e file`
* `[exprn]
	* `[ -e file ]`
* `[[exprn]]
	* `[[ $ ver == 5 ]]`
* ((exprn))
	* `(( $v ** 2 > 10 ))`
* command
	* `wc -l file`
* pipeline
	* `who|grep "joy" > dev/null`
* negation 
	* !condition

## test numeric comparisons
```
$n1 -eq $n2
# check if n1 is equal to n2

$n1 -ge $n2
# greater than or equal 2

$n1 -gt $n2
# greather than

$n1 -le $n2
# less than or equal

$n1 -lt $n2
# less than

$n1 -ne $n2
# not equal to
```


## test string comparison
```
$s1 = $s2
# equality

!=
# not equal

< 
less than

> 
# greater than

-n $s2
# check if str has length great than 0

-z $s2
# check if length of 0
```


# L6.3 - Bash script p1

whenever you execute a file and not source it, it spawns into a subshell, returns the values and then closes itself and none of the variables are accessible even if exported

even if argument has a space within it, can make it one argument using quotations

```
# Example 1
# if do
if test $# -ge 2;
then
        if test $1 = $2;
        then
                echo two arguments are the same
        fi
fi
# Tests is there are at least 2 arguments
and then checks if the first two arguments are the same

# Example 2
# for loop
for i in file_{1..9}
do
        echo $i
done


# Example 3
# list all file sin bin
for i in $(ls /bin)
do
        echo $i
done

# Example 4
# list all files in bin starting with z
for i in $(ls /bin/z*)
do
        echo $i
done
```