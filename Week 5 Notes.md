# L5.1 - Shell Variables
Why use shell environment variables
* communicate between two processes
* file system management becomes intensive, plus there is a security concern

They are available only in the shell or it's subshells

echo commans
```
echo hello, world
# prints a string

echo $HOME
# prints the variable home

# some frequently used shell variables are:
$USERNAME - 
$HOME
$HOSTNAME
$PWD
$PATH

#Shell commands are
printenv
env
set

# Shell variables
$0 - Name of the shell
$$ - process ID of the shell
$? - return code of the previously run program
$- : flags set in the bash shell
```

Convention for shell variables is 
* putting a $ before the name
* All letters being uppercase

## Process Control

* Use of `&` to run a job in the background
* `fg - 
* `coproc
* `jobs
* `top
* `kill`

## Program exit codes
* 0 - success
* 1 -Failure
* 2 - misuse
* 126 - Command cannot be executed
* 127 - command not found
* 180 - process killed using ctrl+C
* 137 - process killed using `kill -9 <pid>`

## Flags set in bash
* h - locate and hash commands
* B - brace expansion enables
* i - interactive mode
* m - job control enables
* H - ! style substituition enabled
* s - commands are read from stdin
* c - commands are read from arguments

## Echo with variables
```
echo $USERNAME # works
echo "$USERNAME" # also works
ECHO '$USERNAME' # prints the string literal
```

## Executing programs
```
# navigate to directory
./date - workds
/date - works
date - works
```


# L5.2 - Shell variables p1

Creation, inspection, modification, lists

## Creating a variables
```
myvar="value string"
```
* can't start with a number
* can mix alphs_numeric chars and _
* no space around =
* `"value string"`can be a number string of command

## Export a variable
* basically make available to access to the processes in the shell
```
export myvar="value string"

or

myvar="value string"
export myvar
```
* Once made can export into the subshell
* But if modified in subshell, doesn't reflect in the parent shell

## Using variable values
```
echo $myvar
echo ${myvar} -> advantage here it becomes like a formatted string from what I understand

echo "${myvar}_something"
```

## Removing a variable
```
unset myvar

or

myvar=
```

## Test if a variable is set
```
# This tests if the variable is set, returns 0 is variable is set
[[ -v myvar ]];
echo $?

# we can also do the opposite
[[ -z ${myvar+x}]];
echo $?
# the above returns 0 is variable is not set
# +x can be any other character
```

## Substitute default value
```
# if myvar is not set, return default
# if it is set, then return it's value
echo ${myvar:-"default"}

# but if you also want to set
echo ${myvar:="default"}
# = is the difference here

# if the variable exists, replace in the variable
echo ${myvar:+default}
# + means that replace if exists, opposite of -
```

## List of variable names
```
echo ${!H*}
# all variables starting with H will be displayed
# the ! is basically saying "give me the names of the variables"
```


## Length of string value
```
echo ${#myvar}
# displays the length of the string value of myvar
# if not set, returns 0
```

## Slice of string value
```
echo ${myvar:5:4}
# the first number is the offset i.e take these many steps and display from there
# the 2nd number is how many steps to display
```

## Pattern matching
### Remove matching pattern
```
echo ${myvar#pattern}
# this only removes the matching pattern once

echo ${myvar##pattern}
# match as many max as possible
```

### Keep matching patterns
```
echo ${myvar%pattern}
# only once

echo ${myvar%%pattern}
# all of them
```

### Replace matching patterns
```
echo ${myvar/pattern/string}
# replace on the first occurence

echo ${myvar//pattern/string}
# replace in all occurences
```

### By location
```
echo ${myvar/#pattern/string}
# match at beginning and replace with string

echo ${myvar/%pattern/string}
# match at the end and replace with string
```

## Changing case
```
echo ${myvar,}
# first char to lower case

echo ${myvar,,}
# change all chars to lower case

echo ${myvar^}
# change first to upper

echo ${myvar^^}
# all to upper
```

## Restricting value types
```
declare -i myvar

# adding restrictions
-i -> add integer
-l -> lowercase chars
-u -> uppercase chars
-r -> read only

# removing restrictions
+i 
+l
+u
+r -> not valid
```

## INDEXED ARRAYS
```
declare -a arr
# declare arr as an indexed array

$arr[0]="value"
# add "value" at index 0

echo ${arr[0]}
# print value of element

echo ${#arr[@]}
# number of elements in the array

echo ${!arr[@]}
# display all indices used

echo ${arr[@]}
# display all values

unset 'arr[2]'

arr+=("value")
# Append an element
```

## ASSOCIATIVE ARRAYS
```
declare -A hash

$hash['a']='value'
# can set any letter or number as an address
```

## Putting output of command into a variable
```
mydate=`date`
# this will store the output of date command in the mydate string
# those are not quotes that is the backtick on the tilde symbol
```


# L5.3 - Shell variables p2
```
example:
myvar=MyFileName.SomethingElse.jpeg

echo ${myvar%%.*}. ${myvar##*.}
out: MyFileName.jpeg

# 4 ways of pattern matching
# first way
echo ${myvar/e/E}
# replaces the small e with big E at first occurence

# second way
echo ${myvar//e/E}
# replaces all e with E

# another way
echo ${myvar/#M/m}
# changes if the first letter is M

# final way
echo ${myvar/%g/G}
# at the end of the string

# example change jpeg to jpg only at the end of the string or the file name
echo ${myvar/%jpeg/jpg}

# capitalization and decapitalization
mydate=`date`
echo ${mydate,}
# will lowercase the first occurence of an uppercase letter

echo ${mydata,,}
# will lowercase the entire string

echo ${mydate^}
# will uppercase the firstl etter

echo ${mydate^^}
# uppercase the entire string
```

data types in shell variables
```
declare -i mynum
mynum=10
echo $mynum
# out will be 10
mynum=hello
echo $mynum
# out will be 0

# can switch off the restriction
declare +i mynum
```

## Arrays
2 types:
* indexed arrays
* associative arrays