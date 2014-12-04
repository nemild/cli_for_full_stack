# CLI for Full Stack Engineers
A prioritized list of the most important CLI commands for full stack engineers

#### Key Principles
* **Less is more**: This is NOT a reference list, it is a prioritized list that totally ignores less used commands or command flags (this is the main reason this document exists)
* **Audience**: The audience is a full stack (or part stack) engineer, NOT a sysadmin
* **Cocktails**: This doc includes command cocktails (multi commands, like a series of piped commands); man pages don’t show this, so this is one reason for this doc (one way to think about this doc is to take the entire histogram of commands used by all developers today - and concentrate on the most important parameterizations of the most important commands)
* **Terseness**: Use the minimal amount of words to explain something, this is not documentation
* **Context**: It might be relevant to explain when or how something should be used if it is non-obvious (or even link to the relevant doc - see strace)
* There’s a mix of BSD and Ubuntu (many people use Mac for dev and a Ubuntu-type instance for the server)
* This doc includes other basics of the CLI (keyboard shortcuts and very basic vim) with the assumption that this helps people get around CLIs (e.g., when ssh-ing into a server)

#### How you can help
* Add additional commands or flags that you think are critical
* Add command cocktails (e.g., multi piped commands) that you think full stack engineers should know or will benefit from
* Rearrange the doc and flags for a given command to be in the order of rough importance/usage

#### General Comments
* Ideally, put general comments on HN, but if you have to you can add them in this section or as comment bubbles
* If you want to debate something or just flame, HN is the best place

##### Authors
* Nemil Dalal (nemild at gmail)
* *[Add your name if you spend more than an hour on this doc]*

## Table of Contents
<a href="#keyboard_shortcuts">Keyboard Shortcuts</a>  
<a href="#getting_around_the_command_line">Getting around the command line</a>  
<a href="#basics">Basics</a>  
<a href="#advanced">Advanced</a>  
<a href="#command_cocktails">Command Cocktails</a>  

<a href="#sysadmin_basics">Sysadmin Basics</a>  
<a href="#vim_basics">Vim (Uber-)Basics</a>  
<a href="#setup">Setup</a>  
<a href="#bash_scripting">Bash Scripting</a>  

## [Keyboard Shortcuts](#keyboard_shortcuts)
Keyboard Shortcut | Description
:-------: | ---------
*Ctrl-C* | terminate program
*Ctrl-A* | go to start of line
*Ctrl-E* | go to end of line
*Ctrl-W* | cut the previous word
*Ctrl-K* | Paste the word
*Ctrl -* | undo
*Ctrl-U* | cut the line
*Ctrl-U/Ctrl-Y* | Cut line, write text and then paste line after new text
*Ctrl-X/Ctrl-E* | use text editor to input command, used for long or multiline commands, can also use `fc`
*Ctrl-L* | clear screen
*Ctrl-D* | terminate program
*Ctrl-Z* | suspend program
*Ctrl-R* | search command history
*ESC-.* | past last argument of previous command
*Ctrl-K* | delete from here to end of line
*Ctrl-B/Ctrl-F* | Page up and down
*Ctrl-D/Ctrl-U* | Half page up and down

## [Getting around the command line](#getting_around_the_command_line)
#### Other Basics
* Three streams are stdin, stdout, and stderr
* find . 2> # the 2> pipes out stderr only, can use &> to pipe out both stdout and stderr

#### Previous Commands or Arguments
```bash
history # see list of previous typed commands
!! # run previous command
!105 # run command number 105 from history

^abc^def # replace first argument with second argument in previous command (only for first occurrence)
!!gs/foo/bar # run last command replacing all foo with bar

!:$ # last argument of previous command
!:0 # first argument of previous command (and number can be incremented for other args)
!string # run most recent argument starting with string
fc # edit your last command in your editor and execute it
```

#### Some basic tricks
```bash
ls \ # the backslash lets you continue a command on the next line
cd abc; ls # the semicolon lets you put separate multiple commands on the same line
cd abc && ls # && means run second command only if first succeeds, compare to ||
\gpom # ignore the alias
 ls # leading space means don’t store in history
```


## [Basics](#basics)

##### Commonly used flags across Linux commands
Flag | Description
:---: | ---
-v | verbose or invert
-i | ignore case
-w | ignore whitespace
-u | user
-h | pretty format the size (from bytes to mb/kb/etc), don’t use this when sorting

#### cd: Change Directory
```bash
cd [ANY DIRECTORY] # base usage
cd # go to home
cd ~ # go to home
cd - # go to previous directory
cd .. # go up one directory
(cd ~/asd && ls) # execute command in subshell, at end you are back in your current directory
there’s also `pushd` and `popd` when you want to push your current location on the stack before moving to the new directory
```

#### mkdir: Make Directory
```bash
mkdir dirname # base usage
mkdir -p dirname1/dirname2 # make all nested directories that don’t exit
rmdir lets you remove an empty directory
```

#### ls: List directory contents
```bash
ls -l # provide information (ownership, permissions, file size, modification date)
ls -h # make disk usage info pretty (kb, mb, etc.) - not bytes
ls -r # reverses order
ls -t # sort by time modified
ls -S # sort files by size
ls -d */ # directories only
ls -p # ack -v # files only
ls -i1 # inode number
```

Popular usages  
`ls -lah`  
`ls -larth`  

#### man: Manual
```bash
man command # shows manual page for command
Ctrl-B/Ctrl-F # page up and down
/searchterm # to search, can use standard vim commands including n and N to go forward and backward to searchterms
```

#### cat: Print a file
```bash
cat -n # number lines starting at 1, can also call nl -b a
cat -b # number non-blank lines, can also call nl -b t
cat -T # Show tabs
Note: Don’t pipe a cat, instead cat < abc.txt
```

#### less: Print a file with pagination
used to print files with pagination, can use standard keyboard shortcuts like page up and down, plus some vim commands like search

```bash
less filename.txt # print file with pagination
less +F # use instead of tail -f, can Ctrl-C, /searchterm, and then type F to have search terms highlighted
```

#### head: Display the first part of a file
```bash
head filename.txt # standard usage
head -n 100 # number of lines
```

#### tail: Display the last part of a file
```bash
tail filename.txt # standard usage
tail -n 5 # last 5 lines
tail -f # don't exit, but keep outputting data as appended (used especially in viewing logs), see the less +F command for a potential replacement
```

#### grep: File Pattern Searcher
```bash
grep abc --color file1.txt file2.txt # can be used with multiple files, abc is substring to search for
grep substring -ri --color . # recursive, ignore case, highlight match in color
grep -v # invert
grep -c # incidence count
grep -x # exact match
grep -A # 3 lines after
grep -B # 3 lines before
grep -C # 3 lines before and after
grep -n # show line numbers
grep -w # search for component words, not the entire substring
grep -l # list files with matching content
grep -E # interpret pattern as a regular expression, can also use egrep (TODO: Add some popular regular expressions people use)
```
TODO Popular combinations  

#### find: Walk a file hierarchy (search for files etc. matching certain criteria, can also be used with xargs/parallel )
```bash
find . -name "hello.txt" # standard usage
find . -name “abc” 2>/dev/null # output to dev null when doing global search, and don't want to discard errors (e.g., file is not accessible because not a super user)
-iname "abc.txt" OR -iname "*.txt" # search for case insensitive
-mtime -7 # modified time is within seven days previous to today
-print0 # used with xargs -0 option, can pipe find results to xargs
-type l/f/d # search for a type of item, f = file, l = symbolic link, d = directory
-not # invert all filtering criteria
find abc/ # lists everything recursively in directory
find -inum 16187430 # inode number
```

#### tar: Archive file or path
```bash
tar -cvf . ~/abc.tar # compress, verbose, compress to filename provided
-xvf # extract, verbose, extract filename provided
-cvzf # gzip as well
```

#### gzip: Compress
```bash
-d # decompress
```

#### sort
```bash
sort # ascending
sort -r # descending, z to a, 10000 to 1
sort -f # ignore case
sort -u # unique only
sort -n # sort according to numerical value
```

#### uniq
Note: only compares adjacent lines for equality, so typically need to sort in advance  

```bash
uniq -c # give incidence of line, will show repeated only once
uniq -d # only output duplicated items, single time
uniq -u # only output lines not repeated
uniq -i # case insensitive
```

#### wc: Word Count
```bash
wc -w # word count only
wc -l # line count only
```

#### ssh: Open SSH client
set the ~/.ssh/config file to make life easier for sshing into commonly used servers

```bash
ssh -i ~/.ec2/abc.pem hello@100.1.24.33 # ssh into server with public key at path
ssh hostname "cd abc && ls -lah" # ssh then run command
ssh -D 1234 hostname # use hostname as a SOCKS proxy on localhost:1234
```

#### mv: Move file from src to dest
```bash
mv src dest # base usage
mv hello.{txt,old} # quick rename
```

#### rm: Remove file
```bash
rm abc.txt # base usage, remove file
rm -r # recursive deletion (including subdirectories)
rm -f # force delete, don’t prompt
rm !(*.foo\|*.bar\|*.baz) # delete everything but
```

#### diff: Compare two files
```bash
-w # ignore all space
-i # ignore case
-E # ignore tab expansion
-a # treat file as text
-C 3 # output num of copied context, 3 lines is default
-U # outpt num of unified context, 3 lines is default
-q # show only that the files differ
-y # side by side
```

#### nl: Line Numbering
is there a reason to use this rather than cat?

```bash
nl -b a # number all lines
nl -b t # number non-empty lines
```

#### scp: Secure Copy
```bash
scp server1:~/abc/def.txt . # base usage
scp -C # compression enable
scp -i abc.pem # use public key
scp -l 5000 # limits bandwith used, Kbit/s
scp -P 21 # remote port
scp -p # preserve modification time
scp -r # recursively copy entire directories
scp -v # verbose mode
```

#### cURL: Transfer a URL
Certain commands taken from [here](http://zaiste.net/2012/11/introduction_to_curl/)
```bash
curl ifconfig.me/ip # get IP
curl ifconfig.me/port # get port
curl -d "birthyear=1905&press=%20OK%20"  http://www.example.com/when.cgi # send data to server, automatically POST, can also separate by separate -d arguments rather than an &
curl -d '{"user": {"name": "zaiste"}}' -H "Content-Type: application/json" http://server/
curl --data-urlencode # 
curl --form upload=@localfilename --form press=OK [URL] |
curl http://www.example.com/index.html -o abc.html # output to file
curl -O http://www.example.com/index.html # output to remote filename on local, in this case index.html
curl -v # verbose
curl -head # head only
```

#### wget: Network Downloader
```bash
wget -O abc.html http://www.example.com # output to file
wget -c http://www.example.com/abc.tar.bz2 # continue a previously interrupted downlaod
wget -i download-file-list.txt # download entire file list, file list has URLs listed one per line
wget --mirror -p --convert-links -P ./localdirectory http://www.example.com/index.html # convert links converts links to local, -p downloads all files to view, -P saves to local directory
```

#### cut: Cut out selected portions of each line of a file
```bash
cut -d':' -f 1,5 # d is delimeter, f is field number, add -s to only show lines with this delimeter (else will print whole line)
cut -c1 # extract character
cut -c1-5 # extract range of characters
cut -c1- # extract from 1 to end
--complement # invert
```

#### tee: copies standard input to standard output, making a copy in file
used when you need to store intermediate output  
```bash
ls | tee abc.txt | something_else
```

#### jq: JSON Command Line Tool
not standard, and yes there could be a large debate of this command, but very useful in today’s world of API’s  

```bash
curl http://www.espn.com/abc.json # jq '.' # no, espn does not have an API
```

#### Other commands
Command | Description
:----: | ----
date / date -r 1231006505 | for unix epoch time
cal / cal 3 1973 | calendar of current month
dirname/basename | basename will strip suffix provided as second argument
whois hostname | 
pidof | 
mcookie | generate pseudo random string
whatis command | short description of command
apropos command | find commands like this
pbcopy / pbpaste | macintosh, pipe to/from these for clipboard, see alias section for Linux
units | convert between units
hostname |
mount /dev/sdb1 /u01 | mount device to directory
read var1 | set variable to stdin

## [Advanced](#advanced)
#### ifconfig: Configure network interface
```bash
ifconfig -a # view all network interfaces
ifconfig eth0 up # turn on
ifconfig eth down # turn off
ifconfig eth0 192.168.2.2
```

#### gcc
```bash
gcc -c abc.cpp # compile cpp
gcc abc.o def.o -o def.out # link and create binary with filename, -o is output
```

#### parallel: Run commands in parallel
```bash
# this is for non-BSD systems
parallel command -- 1 2 3 # each item past the -- is passed to the
command singly, with
-n 3 # send 3 arguments at a time, default is 1
-j 5 # max jobs in parallel
parallel -- ls "cat abc.txt" "df -h" # run specified commands
```

#### xargs: execute multiple arguments
```bash
# run a command on a list of files, used typically from the piped output of another command (e.g. find and ls)
ls | xargs cat # run cat on all files listed from ls
-n 2 # 2 files per call
-I abc # use abc as a value that will be replaced by the filename, sets to 1 file per all when using this
-P 5 # parallel
-0 # add this argument on when using with find -print0
```

#### ldd: print shared library dependencies
```bash
# used to determine shared library dependencies for binaries, useful when attempting to copy a binary from one system to another
ldd -v # verbose
```

#### rsync
TODO

#### pgrep: Find processes by name
```bash
pgrep mysql
pgrep -n # most recent processes
pgrep -n emacs # most recent emacs
pgrep -u bob # only processes started by Bob
pgrep -l # list out name as well, not just PID
pgrep -v # inverts matching
```

#### awk
TODO

#### tr: Translate Characters (replace a character with another)
```bash
cat abc.txt | tr ' ' '\_' # replace
tr "A-Za-z" "a-zA-Z" # reverse case
tr "a-z" "A-Z" # make lowercase uppercase
tr -d '\r' # delete character, useful when moving txt files between Windows and Linux
tr -dc 'a-z A-Z \t \n '\32'' # invert delete
tr -s '\n' # make multiple instance just a single instance
```

#### du: Disk Usage
```bash
du -hs # disk usage of directory, including subdirectories in pretty format
```

#### df: Disk Free
```bash
df -h # disk free by volume, h is pretty format for size (mb, kb, etc)
```

#### time
`time ls # run ls and profile time`  
wall/real time - can be less than some of system + user time, given parallelism  
system time (system calls) vs user time (time spent running user requested command)  

#### sleep
`sleep 5 # sleep for 5 seconds then proceed`

#### sed: Substitution
```bash
sed -e 's/day/night' # replace first day with night per line
sed -e s/day/night/g' # replace all day with night
sed -E # use regular expression
sed -e s/[a-z]/(&)/ # & means use found section
```

#### seq: Sequence
```bash
seq 5 # print sequence of numbers starting at 1
seq -s :abc 5
```

#### mktemp: Create Temp File
```bash
mktemp -t abc # use abc as template
-d # make directory instead of file
# TODO: How to make a file with a certain extension
```

#### watch: Execute a program periodically, showing output fullscreen
```bash
watch -n 5 tail /var/log/messages # runs every 5 seconds
watch -d # highlight differences between successive updates
watch -e # exit if the return value is non-zero
```

#### jot: Generate Data
```bash
jot NUM LOW HIGH STEP
jot -b hello # repeat word
jot -w hello # use as start
jot -r # random
jot -s’:’ # print data with string as separator (not newline)
```

#### lsof: Lists open files for active processes, including pipes and network sockets
```bash
# by default ORs results, not ANDs
# a good tool to be used with strace
# without arguments, lists all open files for default processes
-a # AND the results, this is where the real power comes in
lsof /some/dir/path
lsof somefile.txt
lsof -i # list IP Sockets
lsof -iTCP
lsof -iUDP # UDP is for media, when speed matters, but quality does not
lsof -i :22 # specific port
lso -i@172.16.12.5:21 # Show connections to a specific host
lsof -u username
lsof -u ^username # all users but username
lsof -c top
lsof -p 1232 # for pid
lsof -p `pidof auditd`
lsof -i @fw.google.com:2150=2180 # port range
| grep LISTEN for ports that are awaiting connections
| grep ESTABLISHED
```

#### xxd: convert binary to hex
```bash
xxd somefile.bin # can be used to then compare two binary files with diff
xxd -i somefile.bin # create C array
xxd -r # reverse operation
```

#### strace: Trace system calls and commands (for non-BSD)  
best article on this topic is [here](http://chadfowler.com/blog/2014/01/26/the-magic-of-strace/), dtruss is for Mac  
```bash
-s 2000 # max 2000 characters per call
-F # follow children
strace -p 123 # use PID 123, for running process
-o output.log # output results to file
-c # incidence count by system call, can compare over time
-T time each execution call
# can man most system calls
# can use lsof's FD column to find files
```

#### split: Split file
```bash
split -b 75m input.zip # cut into multiple files of 75m max
cat `ls x*` > reassembled.zip
```

#### dtruss
TODO

#### openssl
```bash
openssl sha1 filename # get sha1 hash of filename
openssl dgst -sha256
openssl dgst -ripemd160
```

[Start of More Advanced?]
#### tcpdump: Dump traffic on a network
```bash
# for a quick primer on TCP look here: TODO
tcpdump -n # don't resolve names, keep IP addresses
-X # displays both hex and ASCII content within the packet
-S # absolute sequence numbers
-s 96 # only takes first 96 by default, can change, 0 is all
tcpdump -nS # Basic communication
tcpdump -nnvvXS
tcpdump -nnvvXSs 1514
tcpdump host 2.3.4.5 # same as both src and dst below
tcpdump src 2.3.4.5
tcpdump dst 2.3.4.5
tcpdump port 3389
tcpdump portrange 21-23
-w capturefile # write to file
-r capturefile # read from file
```

#### nmap: Host Discovery and Port Scanning
```bash
/ uses SYN by default, Discovery mode of host (port 80 is used), then Scan
nmap localhost
nmap 192.16.10.0
nmap -p1-10000 # scan range of ports, default is 1663 ports
nmap -p22,23,10000-15000 192.168.10.0/24 # scan selected ports
nmap -sU -pT:21,22,23,U:53,137 192.168.10.0/24 # for UDP
nmap -sF # FIN scan
nmap -sX # XMAS scan, Christmas Tree packet has all options on, used primarily in scans and DOS
nmap -sA # ACK scan
nmap -sN # NULL scan
nmap -sP abc.com # ping scan, sends ICMP echo and TCP ACK
nmap -p10000 -sV host # gives version information of service on port
nmap -P0 hostname # just scan, don't worry about discovery
nmap -O hostname # tells you Operating System information
nmap -oA # output to all formats below
nmap -oN # .nmap output, human readable
nmap -oG # .gnmap, grepable
nmap -oX # .xml, XML
nmap -resume # can resume when a crash
nmap -vv
/ can vary scan speed
-T Sneaky # 15 seconds, serial
-T Polite # 4 seconds, serial
-T # Aggressive, parallel
```

#### netstat
TODO

#### iostat
TODO

## [Command Cocktails](#command_cocktails)
```bash
grep . *.txt # prints all matched files in directory, and prepends filename to each line
diff <(ls) <(ls) # compare two commands, rather than having to write to a file first
wc < abc.txt # use this instead of cat abc.txt \| wc, saves a new process, important only for big files; don't pipe a cat
(head -5; tail -5) < data # explore first 5 and last 5 lines
head -3 data* | cat # list out first three lines of all files
time read # simple stopwatch
```

## [Sysadmin Basics](#sysadmin_basics)

#### chown: Change ownership (chown)
`chown bob file.txt`

#### chmod: Change mode
read is 4, write is 2, exec is 1  
user (u), group (g), other/everyone else (o), all (a)  
```bash
chmod 755 # rwx, rx, rx
chmod 644 # rw, r, r
chmod +x abc.out
chmod -x abc.out
chmod g+x abc.out
chmod go-rwx file.txt
chmod -R # apply recursively to all files in the subdirectory
```

#### chgrp: Change Group
`chgrp webmasters file.txt`

#### su: Become Super User
```bash
su - johndoe -c 'ls' # run command as user
su - johndoe # switch to user
```

#### passwd: Change password
```bash
passwd # change password
passwd -d johndoe # disable password
```

#### shutdown: Shutdown System
```
shutdown -r now
shutdown now
shutdown -h +10
```

#### top: Display and update sorted information about processes
use htop, not top when possible  
`top -U johndoe`

#### ps: Process status
```bash
ps aux # standard usage, display info about all users processes
ps -u abc,def # users
ps -f -p 123 # find by process id
ps -f -ppid 123 # find by parent process id
ps aux --sort pmem # sort by column
```

#### Other commands
Command | Description
:-----: | -----
groups | Outputs the user groups of which your account belongs to.
adduser johndoe | need super user privileges
passwd -a demo sudo | 
service ssh restart | restart service
finger username | info on user, including logins
last, last username | list of last logged in users
w | users on system, show who is logged in and what they're doing
uptime | time system has been up
free | free space
man hier | get an explanation of the system directory structure
host | get ip of domain


## [vim basics](#vim_basics)
DISCLAIMER: This is just to help someone get the basics of Vim, not for regular users

#### General Notes


#### CLI Commands
Command | Description
:------: | -----
vimtutor | you can type this at the command line to get a Vim textual tutorial
vim filename.txt +25 | open file and go to line 25
vim filename.txt +/abc | open file and go to first occurrence of substring

#### Vim Commands
Key Sequence | Description
:------: | ------
$ | end of line
0 | start of line
w | one word forward
2e | 2 words forward, at end of line
b | before
10j | down ten lines
10k | up ten lines
dw | delete word
u | undo last
U | Undo all on line
dd/p | cut and paste
/ | search for search term going forward
? | search for search term going backward
n | forward when looking for a search term
N | backward when looking for a search term
gg | top of file
G | bottom of file
25G | Goto line
% | go to matching parentheses
3igoESC twice | insert go 3 times
/ # | next/previous of the word that you are at
^F, ^B | page up and page down
:w | save
:wq | save and quit
:q! | quit without saving

## [Setup](#setup)

#### Commonly Used Aliases
```bash
alias ll=”ls -larth”
alias hist='history  | grep'
alias webserver="python -m SimpleHTTPServer"

alias ..="cd .."
alias ...="cd ../.."
alias cd6="cd ../../../../../.."
alias cd5="cd ../../../../.."
alias cd4="cd ../../../.."
alias cd3="cd ../../.."
alias cd2="cd ../.."

alias pbcopy='xclip -selection clipboard' # on Linux to replicate the Mac functionality of command line clipboard
alias pbpaste='xclip -selection clipboard -o' # on Linux to replicate the Mac functionality of command line clipboard
```


## [Bash Scripting](#bash_scripting)

