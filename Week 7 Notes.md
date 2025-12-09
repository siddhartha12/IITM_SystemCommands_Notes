# 7.1 - Bash Scripts - p2A
More features in bash scripts
## Debugging
no explicit debugger, but a small functionality
```
set -x
./myscript.sh

# or

bash -x ./myscript.sh
```
prints the command before executing it
should put the `set -x` inside the script

## Combination Conditions
```
[ $a -gt 3] && [ $a -gt 7 ]
# and conditoin

[ $a -gt 3] || [ $a -gt 7 ]
# or condition
```

using the above, we write a program
```
#!/bin/bash

for i in {1..9};
do
        echo $i
        if [ $i -le 3 ]  || [ $i -ge 7 ]
        then
                echo $i is out of range
        fi
        if [ $i -gt 3 ] && [ $i -lt 7 ]
        then
                echo $i yay
        fi
done
```

the output is as

```
1
1 is out of range
2
2 is out of range
3
3 is out of range
4
4 yay
5
5 yay
6
6 yay
7
7 is out of range
8
8 is out of range
9
9 is out of range
```

## Shell arithmetic
* let
  `let a=$1+5 
  `let "a = $1 + 5"`
	* can use with or without double quotes, if not using double quotes then have to put no spaces
* expr
  `expr $a + 20
  `expr "$a + 20"
  `b = $(expr $a + 20)
	* perform and print the output
	* used to display
* $(( expr ))
  `b=$(( $a + 10))
  `(( b++ ))
	* not returning the value
	* only calculating and storing
* `$[ expr ]
  `b=$[ $a + 10 ]

## Expr command operators
Arithmetic
* +
* -
* *
* /
* %
* >
* <
* >=
* <=
* =
logical
* a | b
	* return a if neither argument is null or 0, else return b
* a & b
	* return a if neither argument is null or 0, else return 0
* a != b
	* 1 is a not equal to b
* `str: reg
	* return the position upto anchored pattern match with BRE str
* match str reg
	* return the pattern match is reg matches pettern in str
* substr str n m
	* return m chars starting from n
* index str chars
	* return position in str where any one of the chars is found else 0
* length str
	* numeric length of string str
* + token
	* interpret token as string even if its a keyboard
* (exprn)
	* return the value of expression


writing an example script
```
#!/bin/bash

if [ $# -lt 2 ]
then
        echo use two natural numbers
        exit 1
fi

re='^[0-9]+$'

if ! [[ $1 =~ $re ]]
then
        echo $1 is not a natural number
        exit 1
fi

if ! [[ $2 =~ $re ]]
then
        echo $2 is not a natural number
        exit 1
fi

let a=$1*$2
echo product a is $a
(( a++ ))
echo product a incremented is $a

c=$[ $1**$2 ]
echo c is $c
```
output is as follows
```
24f1002705@se2001:~/se2001/practice$ ./arithmetic.sh
use two natural numbers
24f1002705@se2001:~/se2001/practice$ ./arithmetic.sh 14 abc
abc is not a natural number
24f1002705@se2001:~/se2001/practice$ ./arithmetic.sh 14 2
product a is 28
product a incremented is 29
c is 196
```

## Heredoc feature
basically don't struggle with long strings which might have some risky characters
```
d = (bc -l << EOF
scale=5
($a+$b)^$c
EOF)
echo #d
# how the normal lines are taken
# but if there are some escape characters or special characters, then the read will become difficult
# so we can create our own end of file character or something similar for temrination of the string and then put as the terminator

d=$(bc -l <<- ABC
	scale=5
	($a+$b)^$c
	ABC
)
echo $d
# the hyphen after << means ignore the tabs in the next lines
ABC is the marker for the end of string
```

# L7.2 - Bash Scripts p2b

## if elif else fi loop
```
if condition1
then
	command
elif condition2
then
	command
elif condition2
then
	command
else
	command
fi
```
Example code
```
#!/bin/bash

if [ $# -gt 2 ]
then
        echo greater than 2
elif [ $# -gt 1 ]
then
        echo less than or equal to 2 but greater than 1, i.e. 2
elif [ $# -gt 0 ]
then
        echo one argument given, giv emore
else
        echo kill yourself
fi
```
it outputs
```
24f1002705@se2001:~/se2001/practice$ ./if.sh
kill yourself
24f1002705@se2001:~/se2001/practice$ ./if.sh 1
one argument given, giv emore
24f1002705@se2001:~/se2001/practice$ ./if.sh 1 2
less than or equal to 2 but greater than 1, i.e. 2
24f1002705@se2001:~/se2001/practice$ ./if.sh 1 2 3
greater than 2
24f1002705@se2001:~/se2001/practice$ ./if.sh 1 2 3 4 5 6
greater than 2
```

## Case statement
```
case $var in
	op1)
		commandset;;
	op2 | op3)
		commandset;;
	op4 | op5 | op6 )
		commandset;;
	*)
		commandset;;
esac
```

## C style for loop: one variable
```
begin=1
finish=10
for (( a=$begin; a < $finish; a++ ))
do
	echo $a
done
```

## C style for loop: two variables
```
begin1=1
begin2=10
finish=10
for (( a=$begin; b=$begin2; a<$finish; a++, b-- ))
do
	echo $a $b
done
```

## Processing output of a loop
```
# loop
do
	commands
done > $filename
```

## break
```
while [ condition ]
do 
	commands
	if [ condition ]
	then
		break
	fi
done
```

break level exists, say you're two loops in, you can say `break 2` to break out of the outer loop

## continue
same as in C
```
while [ condition ]
do 
	commands
	if [ condition ]
	then
		continue
	fi
done

```
the loop where the condition is satisfied will be bypassed


## Shift
shifts the arguments by one to left so like `$2 will become $1, $0 will not change, $1 will be discarded, gone`
* only use when you will no longer need the discarded arugments
* destructive in nature

## exec
```
exec ./myexecutable --myoptions --myargs
```
* to replace shell with a new program or to change io settings
* if new program is launched successfully, it will not return control to the shell
* if new program fails to launch, the shell continues

example:
```
#!/bin/bash
echo PID of the shell running this command
echo $$
echo leaving behind and oprning xterm if available
exec xterm
echo looks like xterm is not available for opening
```


# L7.3 - Bash Scripts p2C

## eval
```
eval my-arg
```
* Execute argument as a shell command
* Combines arguments into a single string
* Returns control to the shell with exit status

Example
```
#!/bin/bash
cmd='date'
fmt='+%d -%B -%Y'
eval $cmd $fmt
```

## functions
example
```
#!/bin/bash

usage()
{
	echo usage $1 str1 str2
	# whatever argument is passed to usage
}

swap()
{
	echo $2 $1
}

if [ $# -lt 2 ]
then
	usage $0 
	# so you're calling usage with the 0th argument of the program as the first argument in usage function
	exit 1
fi

swap $1 $2
```

Can also put some of the functions in an external file. Here's how to do that
```
# mylib.sh
swap()
usage()

# function.sh
source mylib.sh

---rest of the file---
```

## getopts
```
# the ab:c: basically mean to imply that b and c take arguments
while getopts "ab:c:" options;
do
	case "${options}" in
		b)
			barg=${OPTARG}
			echo accepted: -b $barg
			;;
		c)
			carg=${OPTARG}
			echo accepted: -c $carg
			;;
		a)
			echo accepted: -a
			;;
		*)
			echo usage: -a -b barg -c carg
			;;
	esac
done
```
This script can be invoked with only three options: a,b,c. b and c options will take arguments
The output will be the following:
```
24f1002705@se2001:~/se2001/practice$ ./getopts.sh
24f1002705@se2001:~/se2001/practice$ ./getopts.sh -a
accepted: -a
24f1002705@se2001:~/se2001/practice$ ./getopts.sh -a -b barg barg2 -c carg
accepted: -a
accepted: -b barg
24f1002705@se2001:~/se2001/practice$ ./getopts.sh -a -b barg -c carg
accepted: -a
accepted: -b barg
accepted: -c carg
24f1002705@se2001:~/se2001/practice$ ./getopts.sh -a -b barg -c carg -d -e
accepted: -a
accepted: -b barg
accepted: -c carg
./getopts.sh: illegal option -- d
usage: -a -b barg -c carg
./getopts.sh: illegal option -- e
usage: -a -b barg -c carg
```

## select loop
```
#!/bin/bash
echo select middle one
select i in {1..10}
do
	case $i in
		1 | 2 | 3)
			echo you picked a small one;;
		8 | 9 | 10)
			echo big;;
		4 | 5 | 6 | 7)
			echo right
			break;;
	esac
done
echo selection completed with $i
```
The output will be as
```
24f1002705@se2001:~/se2001/practice$ ./select.sh
select middle one
 1) 1
 2) 2
 3) 3
 4) 4
 5) 5
 6) 6
 7) 7
 8) 8
 9) 9
10) 10
#? 4
right
selection completed with 4
24f1002705@se2001:~/se2001/practice$ ./select.sh
select middle one
 11) 1
 12) 2
 13) 3
 14) 4
 15) 5
 16) 6
 17) 7
 18) 8
 19) 9
20) 10
#? 15
#? 34
#? -90
#? 4
right
selection completed with 4
24f1002705@se2001:~/se2001/practice$ ./select.sh
select middle one
 21) 1
 22) 2
 23) 3
 24) 4
 25) 5
 26) 6
 27) 7
 28) 8
 29) 9
30) 10
#? 1
you picked a small one
#? 9
big
#? 4
right
selection completed with 4
```

