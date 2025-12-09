# L8.1 - Automating Scripts
Scheduled, recurring, automatic execution of scripts
## cron
* service to run scripts automatically at scheduled times
* tools:
	* at
	* crontab
	* anacron
	* logrotate
* locations
* /etc/

## Job definition
```
<minute> <hour> <day of month> <month> <day-of-week> username
```
![[Pasted image 20251209214441.png]]

## Startup scripts
* startup scripts:
	* `/etc/init/`
	* `/etc/init.d/`
* Run level scripts
```
/etc/rc0.d/ - Shutdown and power off

/etc/rc1.d/ - single user mode

/etc/rc2.d/ - Non GUI multi use mode w/o networking

/etc/rc3.d/ - Non GUI multi user mode with networking

/etc/rc4.d/ - Non GUI multi user mode for special purposes

/etc/rc5.d/ - GUI multi-user mode with networking

/etc/rc6.d/ - Shutdown and Reboot
```

## Crontab
* crontab has all the scheduled things
* to add we use `crontab -e` from which we get access to a file in which we can setup the routine


# L8.2 - Stream Editor Sed

## Introduction
* is a programming language
* `sed` is an abbreviation for stream editor
* It is a part of POSIX
* sed precedes awk

## Execution model
* input stream is a set of lines
* each line is a sequence of characters
* two data buffers are maintained: active pattern space and auxilary hold space
* for each line of input, an execution cycle is performed loading the line into the pattern space
* During each cycle, all the statements in the script are executed in the sequence for matching address pattern for actions specified with the options provided

## Usage
```
# Single line at the command line
sed -e 's/hello/world/g' input.txt

# Can put all commands in a file and run that also
sed -f ./myscript.sed input.txt
```

## Sed statements
`:label <address-pattern> <action> <options>;`
address-pattern
* address
* address, range
* negation
action
* single character action
* same as 'ed' or 'ex'
options - depend on the action
ends with semicolon

### Grouping comands
`{ cmd1 cmd2 }`

### address
* Selecting by numbers
	* 5 - that line
	* $ - last line
	* % - all lines
	* 1~3 - every 3rd line starting from the first line
* selecting by text matching
	* /regexp/
* range addresses
	* /regexp1/, /regexp2/ - lines from the first occurence of regex1 until the 1st occurence of regex2
	* 5,15 - from 5th to 15th
	* 5,/regexp/ - from 5th to first occurence of regex
	* /regexp/, +4 - from first line of matching to the next 4 lines
	* /regexp/, ~2 - next line multiple of 2

### actions
* p -> print
* d -> delete
* s -> substitue regex match
	* s/pattern/replacement/g
* = -> print current input line number, \n
* `#` -> comment
* i -> insert above current line
* a -> append below current line
* c -> change current line

### programming
```
b label 
# branch unconditionally to label

:label
# specify location of label for branch command

N 
# add a new line to pattern space and append next line to input of it

q
# exit sed without processing anymore commands or input lines

t label
# branch to label only if there was a successful substitution was made

T label
# branch to label only if there was no successful substitution was made

W filename
# write pattern space to filename

x
# exchange the contents of hold and pattern spaces
```

## Bash + sed
* including sed inside shell script
* heredoc feature
* use with other shell scripts on command line using pipe


```
# Example - read a file
sed -e "" sample.txt
# read a file called sample.txt
# default action will be to print

# put -n
sed -n -e "" sample.txt
# -n means perform no action
# doesn't print anything

# print the line numbers
sed -e "=" sample.txt
# prints out the line numbers alond with the newline characters

# pick a line of set of liline
sed -n -e "5p" sample.txt
# the -n is there to say no to printing every other line
# doesn't cancel out the specified line
# the -n is only to suppress default 

sed -n -e '5!p' sample.txt
# the single quotes used here meaning function was passed here
# double would take it as a character

sed -n -e '$p' sample.txt
# basically lines which end in p

sed -n -e '$!p' sample.txt
# lines that do not end in p

sed -n -e '5,8p' sample
# only lines 5-8 will bep rinted

sed -n -e '=, 5,8p' sample
# this will print all line numbers but will print only lines 5 to 8

sed -n -e '5,8{=,p}' sample
# here we see introduction of functions, 
# we specify the line range and then we specify what function to perform on those lines
# so for lines 5 to 0 print their line number and the line also

sed -n -e '1~2p' sample
# prints every odd line

sed -n -e '1~2!p' sample
# prints every even line

sed -n -e '/microsoft/p' sample
# prints every line with mcirosoft in it

sed -n -e '/adobe/,+2p' sample
# prints the line that matches adobe and the 2 lines after

# delete the line and send onto the screen
sed -e '5d'
# deletes the 5th line
sed -e '5,8d'
# deletes the lines from 5 to 8

sed -e '1,$d'
# deletes everything from 1st to last line

sed -e '/micrososft/d'
# deletes all lines that have microsoft

sed -e 's/microsoft/MICROSOFT/g'

sed -e '1s/linux/LINUX/g'
# replace only on 1st line

sed -e '1,$s/in place of/ in lieu of/g'
# in all lines i.e. from 1st to last($)

sed -E -e '3,6s/^L[[:digit:]]+ //g' sample
# -E extended function
# select lines 3 to 6
# replacement function
# Any line starting with L and followed by 
# or any amount of digits
# remove it

sed -E -e '3,/symbolic/s/^L[[:digit:]]+ //g'
# basically select lines from 3 to the first occurence of symbolic
then in those lines, replace the lines starting with L digits with nothing

sed -E -e '/text/,/video/s/^L[[:digit:]]+ //g'
# from the line which is the first occurence of text
# till the line with the first occurence of video
# do the same

sed -e '1i -------------header-----------------' -e '$a-------footer------------' sample
# 1st -e says there is a command
# 1st line i implies insert
# another -e for another command
# $ last line a append

sed -e '/microsoft/c -censored-' sample
# replace every line with censored
```


## SED Files
* files have sed extension
* the header compiler is
	* `#!/usr/bin/sed -f`
* put all the commands in series

```
#!/usr/bin/sed -f
1i --------header---------
$a --------footer---------
1,5s/in place of/in lieu of/g
6i ------simpler stuff here onward-------
6,$s/in place of.*//g

```

DEBUG
`sed -f --debug`