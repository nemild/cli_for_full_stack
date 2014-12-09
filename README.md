# CLI for Full Stack Engineers
A prioritized list of the most important CLI commands for full stack engineers

#### How you can help
* Add additional commands or flags that you think are critical, **especially common usages (e.g., compound arguments) (which don't appear in man pages)**
* **Add command cocktails (e.g., multi piped commands)** that you think full stack engineers should know or will benefit from
* Rearrange the doc and flags for a given command to be in the order of rough importance/usage, and comment if there's something too esoteric in this doc

#### Key Principles
* **Less is more**: This is a prioritized list that totally ignores less used commands or command flags - it is NOT a reference list. This doc is meant to represent the entire histogram of commands used by (or that should be used by) all full stack developers today - and aims to concentrate on the most important parameterizations of the most important commands and command cocktails
* **Audience**: The audience is a full stack (or part stack) engineer, NOT a sysadmin
* **Cocktails**: This doc includes command cocktails (multi commands, like a series of piped commands); man pages don’t show this, so this is a core reason for this doc
* **Terseness**: Use the minimal amount of words to explain something, this is not documentation
* **Context**: It might be relevant to explain when or how something should be used if it is non-obvious (or even link to the relevant doc - see strace)
* There’s a mix of BSD and Ubuntu (many people use Mac for dev and a Ubuntu-type instance for the server) - this probably needs to be better called out
* This doc includes other basics of the CLI (keyboard shortcuts and very basic vim) with the assumption that this helps people get around CLIs (e.g., when ssh-ing into a server)

#### General Comments
* This doc appeared on Hacker News [here](http://news.ycombinator.com/)

##### Authors
* Nemil Dalal (nemild at gmail)
* *[If you spend more than 15 mins on this doc, add your name here]*

## Table of Contents
<a href="#keyboard_shortcuts">Keyboard Shortcuts</a>  
<a href="#getting_around_the_command_line">Getting around the command line</a>  
<a href="#basics">Basics</a>  
<a href="#advanced">Advanced</a>  
<a href="#command_cocktails">Command Cocktails</a>  

<a href="#sysadmin_basics">Sysadmin Basics</a>  
<a href="#vim_basics">Vim (Uber-)Basics</a>  
<a href="#setup">Setup</a>  

## Other Resources
* [Bro Pages](http://bropages.org/): Lists popular options in a man page type format (used to augment several commands below)
* [Command Line Fu](http://www.commandlinefu.com/commands/browse/sort-by-votes): Lists popular commands, use link to see top voted over all time

## [Keyboard Shortcuts](#keyboard_shortcuts)
Keyboard Shortcut | Description
:-------: | ---------
*Ctrl-C* | terminate program
*Ctrl-A* | go to start of line
*Ctrl-E* | go to end of line
*Ctrl-W* | cut the previous word
*Ctrl -* | undo
*Ctrl-U* | clears the line before the cursor position.
*Ctrl-U/Ctrl-Y* | Cut line, write text and then paste line after new text
*Ctrl-XX* | Move between start of line and current cursor position
*Ctrl-X/Ctrl-E* | use text editor to input command, used for long or multiline commands, can also use `fc`
*Ctrl-L* | clear screen
*Ctrl-D* | terminate program
*Ctrl-Z* | suspend program
*Ctrl-R* | search command history, start typing command, ctrl-r again to cycle through other matches in reverse chronological order
*Ctrl-G* | Escape from search command mode (may be just easier to use Ctrl-C)
*ESC-.* | past last argument of previous command
*Ctrl-K* | Kill the line after the cursor
*Ctrl-B/Ctrl-F* | Page up and down
*Ctrl-D/Ctrl-U* | Half page up and down
*Ctrl-S/Ctrl-Q* | stop output to screeen (for verbose commands) / resume output to screen
*Ctrl-T* | swap previous two characters (used for misspellings)
*Meta-T* | swap previous two words

## [Getting around the command line](#getting_around_the_command_line)
#### Other Basics
* Three streams are stdin, stdout, and stderr
* find . 2> # the 2> pipes out stderr only, can use &> to pipe out both stdout and stderr

#### Previous Commands or Arguments
```bash
history # see list of previous typed commands
history | grep command # list out previous calls with command, then do !123 to run where 123 is the number shown for the command
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
-h | pretty format the size (from bytes to mb/kb/etc), if using this when sorting, use sort -h (only non-Mac)
--color | use color

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
ls -G # colorize
ls -S # sort files by size
ls -d */ # directories only
ls -p # ack -v # files only
ls | wc -l # number of items in directory (including subdirectories)
ls -R . #recursively list the files and directories
ls | grep -v *.rb # filter out unwanted results, like .rb files
```

Popular usages  
`ls -lah`  
`ls -larthG`  
`ls -ltr`  

#### man: Manual
```bash
man command # shows manual page for command
Ctrl-B/Ctrl-F # page up and down
/searchterm # to search forward, can use standard vim commands including n and N to go forward and backward to searchterms, use this to search for flags ('-n')
?searchterm # search backward
man ascii # ascii table
man 2 ls # go to section 2 of ls, search for what each section number means
# use with whatis and apropos, to determine what commands available are
# TODO: Any other feedback on using man pages effectively
```

#### cat: Print a file
```bash
cat -n # number lines starting at 1, can also call nl -b a
cat -b # number non-blank lines, can also call nl -b t
cat -T # Show tabs in a file
Note: Don’t pipe a cat ('cat hello | dosomething'), instead dosomething < abc.txt
```

#### less: Print a file with pagination
used to print files with pagination, can use standard keyboard shortcuts like page up and down, plus some vim commands like search. q to quit, arrow keys to move up and down  
```bash
less filename.txt # print file with pagination
less +F # use instead of tail -f, can Ctrl-C, /searchterm, and then type F to have search terms highlighted
```

#### head: Display the first part of a file
```bash

head -100 abc.txt # number of lines, can also do -n 100, default is 10 when not specified
head -c50 # first 50 characters
head abc.txt def.txt # multiple files
head -q *txt *gbk # heads of multiple files w/o delimiters
```

#### tail: Display the last part of a file
```bash
# head flags from above apply here
tail -f # don't exit, but keep outputting data as appended (used especially in viewing logs), see the less +F command for a potential replacement
tail -F # same as -f except If the file is replaced or truncated, follow the new file that is now pointed to by the filename
tail -f "my_server.log" | grep "my process" # Filter lines containing a match for "my process" from a text stream
```

#### echo: Write arguments to the standard output
```bash
echo "line1\nline2" >> .bash_profile # append string to end of .bash_profile, quick way to edit
echo "line1\nline2" > output.txt # quick way to create file with text, compare to touch and then nano/vi
echo $var # print shell variable
echo $(pwd) >> log # way to run a command and append to file
echo "ls -l" | at midnight # execute command at a certain time
echo {a..z} # print a through z, can be used for numbers as well
```

#### ln: Make link (used primarily for symbolic or soft link)
```bash
ln -s /a.out b.out # create symbolic link, a pointer to the original file - use to create a pointer rather than a deep copy, ls -l will show what a symbolic link points to
ln -sb # backup of existing files if overwriting
ln -sf # if target file link exists, unlink so that the new link may occur
```

#### grep: File Pattern Searcher
```bash
grep substring --color file1.txt file2.txt # search for substring can be used with multiple files
grep substring -ri --color . # recursive, ignore case, highlight match in color, a common usage
grep -v # invert
grep -c # incidence count
grep -x # exact match
grep -A # 3 lines after
grep -B # 3 lines before
grep -C # 3 lines before and after
grep -n # show line numbers
grep -w # search for component words, not the entire substring
grep -l # list filenames with matching content
grep -E # interpret pattern as a regular expression, can also use egrep
# TODO: Add some popular regexes for grep at the command line

# Popular usages
grep -iIrn computesSum * # Case insensitive search through all of the files recusively starting from my current directory for the string "computesSum" but does not search binary files.  When a match is found, print the line number it was found on
grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' input.txt > output.txt # grep for IP address
grep -R --include="*.py" "pattern" /path/to/dir # grep recursively through files of a certain type, such as .py files
grep -m 1 "needle" *.txt # show only the first match in each file
```
TODO Popular usages  

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

# Popular usages
find . -name "*.foo" -exec tail {} \; # output the last 10 lines in each .foo file in directory and subdirectories
find . -iname '*foo*' # find all files that contain foo in their name
find . -type d -empty -delete # find and remove empty directories
find . -type f -empty -delete # find and remove empty files
find . -type d -ctime -5 # find all directories created in the last 5 days
find ./ -not -type d -printf "%T+ %p\n" | sort | tail -1 # find last modified file in currenct directory
find -L . -type l # find broken sym links
find ~/chat -name *.txt | xargs grep 'chocolate' # grep for term in each matched file
```
TODO Popular usages  

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
sort abc.txt def.txt # ascending, can do multiple files
sort /path/to/directory/*.csv # sort all files

sort -r # descending, z to a, 10000 to 1
sort -R # randomize
sort -f # ignore case
sort -u # unique only
sort -n # sort according to numerical value
sort -k 2 # start at key 2 (used for seprated columns, like the history command)
sort -h # sort by human readable size (use with -h on another command, like du -h), only usable on non-Mac
```

#### uniq
Note: only compares adjacent lines for equality, so typically need to sort in advance  

```bash
uniq -c # give incidence of line, will show repeated only once
uniq -d # only output duplicated items, single time
uniq -u # only output lines not repeated
uniq -i # case insensitive

uniq -cd # Show count of lines that have duplicates
uniq -C # count number of duplicates
```

#### wc: Word Count
```bash
# by default shows num lines, words, characters (in that order)
wc -w # word count only
wc -l # line count only
wc -L # longest line length, used often for style in code, where 80 lines max may be a desired norm in a team
find path/to/dir | xargs wc -l # count the number of lines of all files in a directory
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
mv srcfile srcfile srcfile destdirectory
mv hello.{txt,old} # quick rename
```

#### rm: Remove file
```bash
rm abc.txt # base usage, remove file
rm -rf abc # recursive deletion (including subdirectories), f is force delete, use with caution
rm -i # prompt before deleting
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
diff -r DIRECTORY1 DIRECTORY2 # show the difference between two directories
diff <(sort file1) <(sort file2) # show the difference between two files
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
# The Basics
curl http://www.example.com/index.html -o abc.html # output to file abc.html
curl -O http://www.example.com/index.html # output to the remote filename on local, in this case index.html

# Common Options
curl -v # verbose

# Sending data
curl -d @invoice.pdf -X POST http://devnull-as-a-service.com/dev/null # post a file
curl -d "birthyear=1905&press=%20OK%20"  http://www.example.com/when.cgi # send data to server, automatically POST, can also separate by separate -d arguments rather than an &
curl http://www.example.com/index.html -o abc.html # output to file
curl -d '{"user": {"name": "zaiste"}}' -H "Content-Type: application/json" http://server/
curl --data-urlencode # 
curl --form upload=@localfilename --form press=OK [URL] |

curl -head # head only

curl ifconfig.me/ip # get IP
curl ifconfig.me/port # get port
```

#### wget: Network Downloader
```bash
wget http://www.example.com/xyz.html # get file and save as xyz.html
wget -O abc.html http://www.example.com/def.html # output to file
wget -c http://www.example.com/abc.tar.bz2 # continue a previously interrupted downlaod
wget -i download-file-list.txt # download entire file list, file list has URLs listed one per line
wget --mirror -p --convert-links -P ./localdirectory http://www.example.com/index.html # convert links converts links to local, -p downloads all files to view, -P saves to local directory
wget -w 2 -r -np -k -p http://www.stanford.edu/class/cs106b # recursively download an entire site, waiting 2 seconds between hits (courtesy Stanford Startup Engineering class)
wget --random-wait -r -p -e robots=off -U mozilla http://www.example.com # Download an entire website

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
ls | tee -a abc.txt # append to the file, rather than overwrite
tee abc.txt def.txt # output to multiple files
```

#### jq: JSON Command Line Tool
not standard, and yes there could be a large debate of this command, but very useful in today’s world of API’s  

```bash
curl http://www.example.com/abc.json # jq '.'
```

#### Other commands
Command | Description
:----: | ----
pwd | print working directory
touch abc.txt | create empty file
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
ping www.google.com | see if a site is up and you can connect to it
yes | print y forever, used if you want dummy text, often used with head to restrict to a certain number of lines, can also tack on a prefix/suffix to make each line unique

## [Advanced](#advanced)
#### ifconfig: Configure network interface
```bash
ifconfig -a # view all network interfaces
ifconfig eth0 # show a single interface
ifconfig eth0 up # turn on
ifconfig eth down # turn off
ifconfig eth0 192.168.2.2 # assign IP address to interface, may need sudo privileges
```

#### gcc
```bash
gcc -c abc.cpp # compile cpp
gcc abc.o def.o -o def.out # link and create binary with filename, -o is output
```

#### [parallel](#parallel): Run commands in parallel
```bash
# this is for non-BSD systems
parallel command -- 1 2 3 # each item past the -- is passed to the
command singly, with
-n 3 # send 3 arguments at a time, default is 1
-j 5 # max jobs in parallel
parallel -- ls "cat abc.txt" "df -h" # run specified commands
# split is used with parallel, as split can break up a file before you farm it out to multiple servers
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
```bash
# used rather than scp when want more complex usage - e.g., can create a directory at the destination if it doesn't already exist
rsync -avz server:abc/ .# recursively transfer all files, using compression (z), plus archive mode (a - preserves symbolic links, dates) and verbose (v)
rsync -avp # preserve permissions
```

#### pgrep: Find processes by name
```bash
pgrep mysql
pgrep -n # most recent processes
pgrep -n emacs # most recent emacs
pgrep -u bob # only processes started by Bob
pgrep -l # list out name as well, not just PID
pgrep -v # inverts matching
pgrep -f abc.rb # Find the pids of processes with 'abc.rb' as an argument, like 'ruby abc.rb'
```

#### awk
[awk one liners](http://www.pement.org/awk/awk1line.txt)

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

#### time
`time ls # run ls and profile time`  
wall/real time - can be less than some of system + user time, given parallelism  
system time (system calls) vs user time (time spent running user requested command)  

#### sleep
```bash
sleep 5 # sleep for 5 seconds then proceed
sleep 5s # sleep for 5 seconds # non-Mac
sleep 5m # sleep for 5 minutes # non-Mac
```

#### sed: Substitution
```bash
sed -e 's/day/night' abc.txt def.txt # replace first day with night per line
sed -e s/day/night/g' # replace all day with night
sed -i # inplace editing
sed -E # use regular expression
sed -e s/[a-z]/(&)/ # & means use found section

# Common usages
sed '/^[[:space:]]*$/d' input.txt > output.txt # remove blank lines from a file
sed -e 's/<[^>]*>//g' index.html # strip HTML
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
watch -n 5 tail /var/log/messages # runs every 5 seconds after previous run completed
watch --precise -n 5 # tries to run 5 seconds from previous start
watch -d # highlight differences between successive updates
watch -e # exit if the return value is non-zero

watch -d ls -l # watch contents of a directory change
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
lsof /some/dir/path # list all open files under path
lsof somefile.txt
lsof -i # list IP Sockets
lsof -iTCP
lsof -i -sTCP:LISTEN # Find listening ports
lsof -i -sTCP:ESTABLISHED # Find established connections
lsof -iUDP # UDP is for media, when speed matters, but quality does not
lsof -i :80 # list all processes on specific port, this is for web processes
lso -i@172.16.12.5:21 # Show connections to a specific host
lsof -i @192.168.1.2:6881=6887 # Show open connections within a port range
lsof -u username
lsof -u ^username # all users but username
lsof -c top # show files/connections open by a binary
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
This command is often used in concert with [paralle](#parallel): split breaks up a file (like a data CSV), and then parallel lets you process each section concurrently
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
openssl s_client -connect www.domain.com:443 -showcerts # get certificate from a certain server
```

[Start of More Advanced?]
#### tcpdump: Dump traffic on a network
(Note: This needs to be culled)
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
(Note: This needs to be culled)
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

## [Command Cocktails](#command_cocktails)
```bash
grep . *.txt # prints all matched files in directory, and prepends filename to each line
diff <(ls) <(ls) # compare ouput of two commands, rather than having to write each to a file first
wc < abc.txt # use this instead of cat abc.txt \| wc, saves a new process, important only for big files; don't pipe a cat
(head -5; tail -5) < data # explore first 5 and last 5 lines
head -3 data* | cat # list out first three lines of all files
time read # simple stopwatch, Ctrl-C to stop
```

## [Sysadmin Basics](#sysadmin_basics)
This section is for things related to user management, filesystem, or the general system.

#### sudo
```bash
sudo command # become a root temporarily (to execute a command), prompts for password when first typed and then won't ask for password again for a 5 min window during that terminal session
sudo !! # run previous command as sudo

```

#### Package management - apt-get, brew, and install from source
```bash
# apt-get is the main package manger for Ubuntu Linux
sudo apt-get install -y python-software-properties python g++ make # install all of the following packages without prompting for confirmation
sudo apt-get remove g++ # remove package
sudo apt-get update # update list of packages available
```

```bash 
# Homebrew is a popular package manager for Mac, can also use port
brew install formula # install a formula, you should not have to use sudo with brew
brew remove formula
brew update formula
brew search formula
```

```bash
# can also install from source, this is the preferred way so you are not clobbering an existing package, and so it is easy to delete if needed
./configure --prefix=$HOME/my-sqlite-dir # pick whatever name you want
make
make install

# else, you can do this
./configure
make
sudo make install
```

#### chown: Change ownership (chown)
```bash
chown bob file.txt
chown -R user:group path/to/directory/ # apply recursively
```

#### chmod: Change mode
read is 4, write is 2, exec is 1  
user (u), group (g), other/everyone else (o), all (a)  
```bash
chmod 755 # rwx, rx, rx
chmod 644 # rw, r, r
chmod ugo+rwx abc.out # this commands shows everything at once, can also use minus (-)
chmod +x abc.out
chmod -x abc.out
chmod g+x abc.out
chmod go-rwx file.txt
chmod -R # apply recursively to all files in the subdirectory

sudo chmod -R 700 secret_folder # allow only root access to folder
```

#### chgrp: Change Group
`chgrp webmasters file.txt`

#### su: Become Super User
```bash
sudo su # switches you to root user, use carefully
su - johndoe -c 'ls' # run command as user
su - johndoe # switch to user
```

#### du: Disk Usage
```bash
du -hs * # disk usage of directory, including subdirectories in pretty format
du -h --max-depth=1 # List human readable size of all sub folders from current location

du --max-depth=1 -b | sort -k1 -rn # largest files in directory, linux only, not Mac
```

#### df: Disk Free
```bash
df -h # disk free by volume, h is pretty format for size (mb, kb, etc)
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
ps aux | grep process # standard usage, display info about all users processes
ps -u abc,def # users
ps -f -p 123 # find by process id
ps -f -ppid 123 # find by parent process id
ps aux --sort pmem # sort by column
```

#### Other commands
Command | Description
:-----: | -----
which command | determine which command path is used to run command, used when you may have multiple binaries
env | print system environment variables
export PATH=$HOME/bin:$PATH | add bin path to the existing path
groups | Outputs the user groups of which your account belongs to.
adduser johndoe | need super user privileges
passwd -a demo sudo | 
service ssh restart | restart service
whoami | Prints your username
finger username | info on user, including logins
last, last username | list of last logged in users
w | users on system, show who is logged in and what they're doing
uptime | time system has been up
free | free space
man hier | get an explanation of the system directory structure
host | get ip of domain
uname -a | get information about system


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
alias hist='history  | grep'
alias webserver="python -m SimpleHTTPServer"

alias ..="cd .."
alias ...="cd ../.."
alias cd6="cd ../../../../../.."
alias cd5="cd ../../../../.."
alias cd4="cd ../../../.."
alias cd3="cd ../../.."
alias cd2="cd ../.."

# these are courtesy of Stanford's Startup Engineering class
alias ll="ls -alrtF --color"
alias la="ls -A"
alias l="ls -CF"
alias dir='ls --color=auto --format=vertical'
alias vdir='ls --color=auto --format=long'
alias m='less'
alias md='mkdir'
alias cl='clear'
alias du='du -ch --max-depth=1'
alias treeacl='tree -A -C -L 2'

alias em='emacs -nw'     # No X11 windows
alias eqq='emacs -nw -Q' # No config and no X11

export GREP_OPTIONS='--color=auto'
export GREP_COLOR='1;31' # green for matches

# Ensures cross-platform sorting behavior of GNU sort.
# http://www.gnu.org/software/coreutils/faq/coreutils-faq.html#Sort-does-not-sort-in-normal-order_0021
unset LANG
export LC_ALL=POSIX

alias pbcopy='xclip -selection clipboard' # on Linux to replicate the Mac functionality of command line clipboard
alias pbpaste='xclip -selection clipboard -o' # on Linux to replicate the Mac functionality of command line clipboard
```