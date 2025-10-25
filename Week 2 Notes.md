# L2.1 - Command line editors p1
Types of editors
* Line editors
	* ed
	* ex
* terminal editors
	* pico
		* nano
	* vi - most acomplex
	* emacs - alternative
* GUI
	* KDE 
		* Kate
		* kwrite
	* Gnome
		* gedit
	* Sublime
	* atom
	* brackets
* IDE
	* eclipse
	* bluefish
	* netbeans

vi and nano come built in with linux

features:
* scroll, view modes, current position in file
* navigation (char, word, line pattern)
* insert, replace, delete
* cut-copy-paste
* search-replace
* language-aware syntax highlight
* key-maps, init-scripts, macros
* plugins

## Writing to file:
can use echo, here we are using echo function
* echo "text" > filename
to append:
* echo "text" >> filename


## ed editor
used in vi editor, so best to know a little, even if vi doesn't exist, ed will exist in unix kernels
* P  - Show prompt
* Command format
	* ```
	  [addr[,addr]]cmd[params]
	  ```
* commands for location
	* no -> refers to the number of line
	* . -> current line
	* $ -> last line
	* % -> all lines
	* + -> after cursor
	* - -> before cursor
	* , -> refers to the entire buffer
	* ; ->from current position to the end of the file
	* /RE/ -> re is regular expression where the expression can be located in the file
* !command - to execute shells script
* e *filename*  -> edit a file
* r *filename* -> read file contents into the buffer
* r !*command* -> read command output into a file
* w *filename* -> write buffer to filename
* q -> quit
* Commands for editing:
	* f, p, a, c, d, i, j, s, m, u
	* p -> print
	* .d -> delete
	* a -> append
	* . -> on a new line and enter, will exit out of the append mode and save it to the buffer
	* s/*find*/*replace*/ -> find word and replace it with the replace word, gonna be active on the line the cursor is at
	* f -> will print the filename
	* j -> join lines (5,6j) -> will join lines 5 and 6
	* m -> commanded as m*number* the current line will move to that position
	* u -> undo
	* c -> change line
	* i -> insert line at current position

Workflow
* start with ed *filename* (enters editor)
* it will simply return a number which represents the size of the file, for it to take prompts enter P which will start showing a * next line which is the place to prompt
* to add outputs into the buffer, use r
* then we need to save the file from the buffer, use w

To add something before all the lines
```
%s/\(.*\)/PREFIX \1/
```

# L2.2 - Command Line Editors - p2
pico and nano are pretty much the same file
![[Pasted image 20251024153226.png]]![[Pasted image 20251024153307.png]]
![[Pasted image 20251024161109.png]]
* i -> insert character
* o -> insert line
* a -> append

![[Pasted image 20251024161217.png]]

![[Pasted image 20251024161343.png]]
![[Pasted image 20251024161409.png]]

![[Pasted image 20251024161428.png]]

# 2.3 - Command line editors p3
## vi editor commands
i -> insert mode
r -> replace letter by letter
dw -> delete word
dd -> delete line
y -> copy (yyy -> yank 3 lines)
p -> print
:*no* -> go to line number
^ g -> to see number of lines

## Emacs Editor

![[Pasted image 20251024165542.png]]

![[Pasted image 20251024165616.png]]

# L2.4 - Networking commands and SSH
3 levels of private networks
## Ports
Like different window for different actions like in a bank

### Ways to gain remote access
* VPN access
* ssh tunneling
* remote desktop
* desktop over browser: Apache Guacamole
* Commercial over internet: TeamViewed, AnyDesk, Zoho assist

### Some important ports
* 21 - ftp
* 22 - ssh
* 25 - smtp, simple mail transfer protocol
* 80 - http, HyperText Transfer Protocol
* 443 - Secure http, https
* 631 - cups, Common Unix Printing System
* 3306 - mysql database

## FireWall
* ports open on my machine
* ports needed to be accessed on remote machine
* Network routing over the port
* firewall controls at each hop

## Protecting a server
* Network firewall
* Web Application Filter

## SELinus
* SEcurity enhanced linux, recommended for all public facing servers
* available in server grade 
* Additional layer of access control on files to services
* role based access control
* Process sandboxing

## Network tools
* ping - to see if machine is up
* traceroute - diagnositcs the hop timings to remote machine
* nslookup - ask for conversion of ip address to name
* dig - DNS lookup utility
* netstat - print network connections
* mxtoolbox.com - for help with accessibility from public network
* whois lookup - who owns which domain name
* nmap - network port scanner
* wireshark - network protocol analyser

just watch the lecture, went over my head tbh

