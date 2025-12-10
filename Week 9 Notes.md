# L9.1 - AWK Programming p1

A language for processing fields and records

## Introduction
* although viewed as a scripting language, has enough math to be a programming language
* abbreviation of Aho, Weingerber and Kerninghan
* It is a part of POSIX
* variants: nawk, gawk, mawk

## Execution Model
* Input stream is a set of records
	* using \n as a record separator, lines are records
* Each record is a sequence of fields
	* eg; using "" as field separator, words are fields
* splitting of records to fields is done automatically
* Each code block executes on one record at a time, as matched by the pattern of that block

## Usage
Calling on command line
```
# Single line command
cat /etc/passwd | awk -F":" `{print $1}`

# script interpreted by AWK
./myscript.awk /etc/passwd
```

 file structure
 ```
 #!/usr/bin/gawk -F
 BEGIN {
	 FS=":" # field separator
 }
 pattern{
	 print $1
 }
 END {
	 print ...
 }
 ```
 * Before processing the line, the being part is done
 * for every line in the file, the default section is executed
 * end is done after the file
All begin blocks are processed at the beginning and all end blocks are processed at the end, the order in each section is determined by the order typed into the document

## Built-in Structure
* ARGC 
	* number of arguments supplied on the command line excluding -f & -v
* ARGV
	* array of command line arguments supplied
* EVIRON
	* associate array of environment variables
* FILENAME
	* file being processed
* FNR
	* number of the current record relative to the current file
* FS
	* field separation, can use regex
* NF
	* number of fields in the current record
* NR
	* Number of the current record
* OMFT
	* output format for numbers
* OFS
	* Output field separator
* ORS
	* Output Record Separator
* RS
	* Record separation
* RLENGTH
	* length of string matched by match() function
* RSTART
	* first position in the string matched by match() function
* SUBSEP
	* Separator character for arrays subscripts
* $0
	* entire input record
* $n
	* nth field in the current record

## AWK Script structure
```
pattern {procedure}
```
Pattern can be:
* BEGIN
* END
* regular expression (**REGEX**)
* Relational expression
* Pattern-matching expression

Procedure is
* variable assignment
* array assignment
* Input/output commands
* Build-in functions
* User-defined functions
* Control Loops

## Operators
* expr ? a : b - conditional expressiog
* a in array - array membership
* a ~ /regex/ - regular expression match
* a !~ /regex/ - Negation
* ++ - increment post and pre fix
* -- - decrement post and pre fix
* $ - field reference
*    - blank is for concatenation

## Functions and commands
Arithmetic
* atan2
* cos 
* exp
* int
* log
* rand
* sin
* sqrt
* srand
String
* asort
* asorti
* gsub
* index
* length
* match
* split
* sprintf
* strtonum
* sub
* substr
* tolower
* toupper
Control
* break
* continue
* do
* while
* exit
* for
* if
* else
* return
input/output
* close
* fflush
* getline
* next
* nextline
* print
* printf
Programming
* Extension
* delete
* function
* system
bit-wise
* and 
* compl
* lshift
* or
* rshift
* xor

## Examples
```
#!/usr/bin/gawk -f
BEGIN {
	print "begin action is processed"
}
{
	print "record:" $0;
	print "processing record number: " FNR;
	print "number of fields in the current reocrd: " NF;
}
END {
	print "end action processed"
}
```

based on character type
```
/[[:alpha:]] {
	commands
}

/[[:digit:]] {
	commands
}
```

Only the first field of the line
```
$1 ~ /[[:alpha:]] {
	print "alpha";
}

$1 ~ /[[:alnum:]] {
	print "alnum";
}

$1 ~ /[[:digit:]] {
	print "digit";
}
```
with multi type fo separators and number of fields
```
#!/usr/bin/gawk
BEGIN {
	print "BEGIN";
	FS="[ .;:-]"
	# any one of the above is valid
}
NF > 2 {
	print "acceptable";
}
NF <= 2 {
	print "not acceptable";
}
END {
	print "END";
}
```

# L7.2 - AWK p2
## Arrays
* Associative
* sparse storage
* Index need not be integer
* `arr[index] = value
* `for (var in arr)
* `delete arr[index]`

## Loops
* `for (a in array)`
* `if (a > b)`
* `for (i=0;i<n;i++)
* `while (a<n)`
* `do {} while (a<n)`

## Pretty printing
Basically printf from C, exactly the same

