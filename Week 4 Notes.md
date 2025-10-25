# L4.1 - Pattern matching p1
stick to posix expressions
## regex 
is a pattern template to filter text
* BRE: POSIX Basic Regular Expression engine
* ERE: POSIX Extended Regular Expression engine
### Why regex?
* languages: java, perl, python, ruby
* Tools: grep, sed awk
* Applications: MySQL, PostgreSQL
We would need to perform input operations or some string operations in some of the scripting languages. Regular expressions are used to perform these operations without being tied down to the actual value of string.

### Usage
* grep 'pattern' filename
* command | grep 'pattern'
Default engine: BRE
To switch to ERe
* egrep 'pattern' filename
* grep -E 'pattern' filename

### Special characters (BRE and ERE)
* . -> any single character except null and newline
* * -> zero or more of the preceding character/expression
* [] -> any of the enclosed characters; hyphen indicates character range
* ^ anchor for begining of line or negation of enclosed characters
* $ -> anchor for end of line
* \ -> escape special characters

![[Pasted image 20251025173517.png]]
![[Pasted image 20251025173558.png]]
![[Pasted image 20251025173705.png]]
![[Pasted image 20251025173814.png]]

### Grep code
* the grep command will return the lines with the matching pattern
```
# can execute in the following ways
grep 'ai' names.txt

# can also use cat command
cat names.txt | grep 'ai'
# will output the same, the out of the cat is the input for the grep 

grep 'S.n'
# which means S followed by any character followed by n

grep '.am'
# where followed by any character with am

grep '.am$'
# where .am is at the end of the line

grep '\.'
# where the initial is there like letter followed by period

grep '.\.'
# if we use dot it's for any character, using the backslash, we look for the actual dot in the text, it's an escape character
# here we need any letter before the dot

grep '^E'
# where lines starting with E

grep -i '^e'
# case insensitive matching

grep 'am\b'
# this is when it is at the end of the word, not just the line. for the line its the dollar sig

grep 'M[ME]'
# first character is matched with M and the second character can either be M or E

grep [ME]E
# either matches with ME or EE

grep 'S.*[mn]'
# starts with a capital S, followed by any character, the asterisk makes it any number of characters, ends with m or n, but this can select entire strings

grep '\bS.*[mn]'
# we introduce a word boundary such that it will only be the starting of a word

grep [aeiou][aeiou]
# basically where you find any two vowels beside each other

grep 'B90[1-4]'
# basically show sequence of B901 - B904

grep '[M-Z][aeiou]'
# Basically where first letter is in the second half of the laphabet followed by a vowel

grep 'B90[^1-4]'
# now it outputs that are not 901-904

grep 'M\{2\}'
# shows where MM is matching

grep 'M\{1,2\}'
# shows wherever there is M or MM
# brace is supposed to have backslash or else it'll search for the character,
# between each of the brace we give the number

grep '\(ma\).*\1'
# here
# once again parenthese need backslash so they are escaped,
# saying look for ma, then any number of characters
# then we use \1 to refer to the first group, here which is ma

grep '\(.a\).*\1'
# here basically any two groups of letters which has the second letter as a with any number of characters in between

grep '\(a.\).*\{3\}'
# once again group os a followed by any character
# it should be repeated thrice with any number of characters between thr groups

grep '\(a.\).*\{2,3\}'
# same as above but 2 or 3 times
```

### egrep code
```
egrep 'M+'
# this here means either one occurence of M or MM

egrep '^M*'
# This will just return all lines

egrep 'M*a'
# basically either Ma or wherever there is a letter before a (very confusing)

egrep 'M.*a'
# this will show groups of M...a

egrep '(ma)+'
# either ma or mama it will match

egrep '(Anu|Raman)'
# either any or raman

egrep '(am|an)$'
# at the end of the line
```

# L4.2 - Pattern matching p2
```
dpkg-query -W -f'${Section} ${binary:Package}\n' | egrep ' .{4}$'

# -w to provide info about packages
# -f for format inside the apostrophes
# the space inside apostrophes for egrep is to say that the name is after a space
# the braces to say that it should repeat 4 times
# we will get package names that are exactly 4 characters long
# If the dollar is not there, we get with 4 or more characters

dpkg-query -W -f'${Section} ${binary:Package}\n' | egrep 'g.{3}$'
# we get packages startingn with g and having length 4 at the end of the line


dpkg-query -W -f'${Section} ${binary:Package}\n' | egrep 'g.{1,5}$'
# where there is either 1 character after g to 5 characters after g

dpkg-query -W -f'${Section} ${binary:Package}\n' | egrep ' k.*$*
# packages that start with k
```

```
cat chartypes.txt | grep '[[:alpha:]]'
# returns any line with alphabet in it

cat chartypes.txt | grep '[[:alnum:]]'
# alphanumeric

cat chartypes.txt | grep '^[[:alnum:]]'
# starts with alphanumeric

cat chartypes.txt | grep '[[:alnum:]]$'
# ends with alphanumeric

cat chartypes.txt | grep '^[[:digit:]]'
# starts with digit

cat chartypes.txt | grep '[[:cntrl:]]'
# has control characters

rest all refer to the screengrab above
```

## Trimming text - cut
```
cut -c 1-4 fields.txt
# -c is the number of characters
# will return the first 4 characters of every line

cut -c -4 fields.txt
# assumes from begining

cut -c 9- fields.txt
# from 9th till the end

cat fields.txt | cut -d " " -f 1
# -d is delimiter
# -f refers to the fields, we are asking only for the first field

# if format is as follows
# <field1>;<field2>,<field3>
# how to get field2
cat fields.txt | cut -d ";" -f 2 | cut -d ',' -f 1
# basically printing the text from fields into the first cut function
it splits into <field1> and field 2 becomes <field2 and 3>
# then it is piped into the cut with delimiter as , which splits the field2 and field3
# then we choose the first field from there

# say from above you want to pick the 2nd line
cat fields.txt | cut -d ";" -f 2 | cut -d ',' -f 1 | head -n 2 | tail -n 1
```

# L4.3 - What is find?
```
find .
# . signifies current directory
# shows all files and directories
# looks through subdirectories also

find <directory_name>
# shows the above in that folder

find -type d
# shows only directories

find -type f
# shows files

find . -type f -name "test1.txt"
# searches for test1.txt in the current folder

find . -type f -name "test*"
# returns files that start with test

find . -type f -iname "test*"
# case insensitive

find . -type f -iname "*.py"


# starts with m -> modified
find . -type f -mmin -10
# shows files modified less tha 10 mins ago

find . -type f -mmin +10
# more than 10 mins ago

find . -type f -mmin +1 -5
# more than 1 mins ago less than 5

find . -type f -mtime -10
# less than 20 days ago

# starts with a -> access (amin, atime)
# starts with c -> changed (cmin, ctime)

find . -size +5M
# returns files over 5 megabytes
# k-> kilobytes
# G -> gigabytes

find . -empty
# empty files

find . -perm 777
# for permissions files which have 777
```

Task-1
```
find <directory-name> -exec <command>
```

Task 2 - set all folders to 775 and files to 664
```
find Website_name -type d -exec chmod 775{} +

find Website_name -type f -exec chmod 664 {} +

# + here is the terminator for the line
```

Task 3 - delete all jpgs
```
find . -type f -name '*.jpeg' -maxdepth 1 -exec rm {} +
# here we use the maxdepth to prohibit find from finding files in subdirectories and only in the specified directory
```