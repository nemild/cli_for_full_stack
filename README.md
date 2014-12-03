# CLI for Full Stack Engineers
Goal of this doc: Prioritized list of the most important CLI commands for full stack engineers

#### Key Principles
* **Less is more**: This is NOT a reference list, it is a prioritized list that totally ignores less used commands or command flags (this is the main reason this document exists)
* **Audience**: The audience is a full stack (or part stack) engineer, NOT a sysadmin
* **Cocktails**: This doc includes command cocktails (multi commands, like a series of piped commands); man pages don’t show this, so this is one reason for this doc
* **Terseness**: Use the minimal amount of words to explain something, this is not documentation
* **Context**: It might be relevant to explain when or how something should be used if it is non-obvious
* There’s a mix of BSD and Ubuntu (many people use Mac for dev and a Ubuntu-type instance for the server)
* This doc includes other basics of the CLI (keyboard shortcuts and very basic vim) with the assumption that this helps people get around CLIs (e.g., when ssh-ing into a server)

#### How you can help
* Add additional commands or flags that you think are critical
* Add command cocktails (e.g., multi piped commands) that you think full stack engineers should know or will benefit from
* Rearrange the doc and flags for a given command to be in the order of rough importance/usage
* PLEASE USE A COLOR OTHER THAN BLACK WHEN ADDING, SO WE CAN TRACK ADDITIONS

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
```bash
ls \ # the backslash lets you continue a command on the next line
cd abc; ls # the semicolon lets you put separate multiple commands on the same line
cd abc && ls # && means run second command only if first succeeds, compare to ||
\gpom # ignore the alias
 ls # leading space means don’t store in history
```

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

#### Other Basics
* Three streams are stdin, stdout, and stderr
* find . 2> # the 2> pipes out stderr only, can use &> to pipe out both stdout and stderr


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
Command | Description
:----: | ----
cd [ANY DIRECTORY] | base usage
cd | go to home
cd ~ | go to home
cd - | go to previous directory
cd .. | go up one directory
(cd ~/asd && ls) | execute command in subshell, at end you are back in your current directory
there’s also `pushd` and `popd` when you want to push your current location on the stack before moving to the new directory

#### mkdir: Make Directory
Command | Description
:----: | ----
mkdir dirname | base usage
mkdir -p dirname1/dirname2 | make all nested directories that don’t exit
rmdir lets you remove an empty directory

#### ls: List directory contents
Command | Description
:----: | ----
ls -l | provide information (ownership, permissions, file size, modification date)
ls -h | make disk usage info pretty (kb, mb, etc.) - not bytes
ls -r | reverses order
ls -t | sort by time modified
ls -S | sort files by size
ls -d */ | directories only
ls -p | ack -v | files only
ls -i1 | inode number

Popular usages
`ls -lah`  
`ls -larth`  

#### man: Manual
Command | Description
:----: | ----
man command | shows manual page for command
Ctrl-B/Ctrl-F | page up and down
/searchterm | to search, can use standard vim commands including n and N to go forward and backward to searchterms

#### cat: Print a file
Command | Description
:----: | ----
cat -n | number lines starting at 1, can also call nl -b a
cat -b | number non-blank lines, can also call nl -b t
cat -T | Show tabs
Note: Don’t pipe a cat, instead cat < abc.txt

#### less: Print a file with pagination
used to print files with pagination, can use standard keyboard shortcuts like page up and down, plus some vim commands like search  
Command | Description
:----: | ----
less filname.txt | print file with pagination
less +F | use instead of tail -f, can Ctrl-C, /searchterm, and then type F to have search terms highlighted

#### head: Display the first part of a file
Command | Description
:----: | ----
head filename.txt | standard usage
head -n 100 | number of lines

#### tail: Display the last part of a file
Command | Description
:----: | ----
tail filename.txt | standard usage
tail -n 5 | last 5 lines
tail -f | don't exit, but keep outputting data as appended (used especially in viewing logs)

#### grep: File Pattern Searcher
Command | Description
:----: | ----
grep abc file1.txt file2.txt | can be used with multiple files, abc is substring to search for
-r | recursive (all files in directory)
-v | invert
-i | ignore case
-c | incidence count
-x | exact match
-A | 3 lines after
-B | 3 lines before
-C | 3 lines before and after
-n | show line numbers
-w | search for component words, not the entire substring
TODO Popular combinations  

#### find: Walk a file hierarchy (search for files etc. matching certain criteria, can also be used with xargs/parallel )
Command | Description
:----: | ----
find . -name "hello.txt" | standard usage
find . -name “abc” 2>/dev/null | output to dev null when doing global search, and don't want to discard errors (e.g., file is not accessible because not a super user)
-iname "abc.txt" OR -iname "*.txt" | search for case insensitive
-mtime -7 | modified time is within seven days previous to today
-print0 | used with xargs -0 option, can pipe find results to xargs
-type l/f/d | search for a type of item, f = file, l = symbolic link, d = directory
-not | invert all filtering criteria
find abc/ | lists everything recursively in directory
find -inum 16187430 | inode number

#### tar: Archive file or path
Command | Description
:----: | ----
tar -cvf . ~/abc.tar | compress, verbose, compress to filename provided
-xvf | extract, verbose, extract filename provided
-cvzf | gzip as well

#### gzip: Compress
Command | Description
:----: | ----
-d | decompress

#### sort
Command | Description
:----: | ----
sort | ascending
sort -r | descending, z to a, 10000 to 1
sort -f | ignore case
sort -u | unique only
sort -n | sort according to numerical value

#### uniq
Note: only compares adjacent lines for equality, so typically need to sort in advance  

Command | Description
:----: | ----
uniq -c | give incidence of line, will show repeated only once
uniq -d | only output duplicated items, single time
uniq -u | only output lines not repeated
uniq -i | case insensitive

#### wc: Word Count
Command | Description
:----: | ----
wc -w | word count only
wc -l | line count only

#### ssh: Open SSH client
set the ~/.ssh/config file to make life easier for sshing into commonly used servers

Command | Description
:----: | ----
ssh -i ~/.ec2/abc.pem hello@100.1.24.33 | ssh into server with public key at path
ssh hostname "cd abc && ls -lah" | ssh then run command

#### mv: Move file from src to dest
Command | Description
:----: | ----
mv src dest | base usage
mv hello.{txt,old} | quick rename

#### rm: Remove file
Command | Description
:----: | ----
rm abc.txt | base usage, remove file
rm -r | recursive deletion (including subdirectories)
rm -f | force delete, don’t prompt
rm !(*.foo\|*.bar\|*.baz) | delete everything but

#### diff: Compare two files
Command | Description
:----: | ----
-w | ignore all space
-i | ignore case
-E | ignore tab expansion
-a | treat file as text
-C 3 | output num of copied context, 3 lines is default
-U | outpt num of unified context, 3 lines is default
-q | show only that the files differ
-y | side by side

#### nl: Line Numbering
is there a reason to use this rather than cat?

Command | Description
:----: | ----
nl -b a | number all lines
nl -b t | number non-empty lines

#### scp: Secure Copy
Command | Description
:----: | ----
scp server1:~/abc/def.txt . | base usage
scp -C | compression enable
scp -i abc.pem | use public key
scp -l 5000 | limits bandwith used, Kbit/s
scp -P 21 | remote port
scp -p | preserve modification time
scp -r | recursively copy entire directories
scp -v | verbose mode

#### curl: Transfer a URL
Command | Description
:----: | ----
curl ifconfig.me/ip | get IP
curl ifconfig.me/port | get port
curl --data "birthyear=1905&press=%20OK%20"  http://www.example.com/when.cgi | 
curl --data-urlencode | 
curl --form upload=@localfilename --form press=OK [URL] |
-o abc.html | output to file
-v | verbose
-head | head only

#### wget: Network Downloader
Command | Description
:----: | ----
wget -O abc.html http://www.microsoft.com | output to file
wget -i download-file-list.txt | download entire file list

#### cut: Cut out selected portions of each line of a file
Command | Description
:----: | ----
cut -d':' -f 1,5 | d is delimeter, f is field number, add -s to only show lines with this delimeter (else will print whole line)
cut -c1 | extract character
cut -c1-5 | extract range of characters
cut -c1- | extract from 1 to end
--complement | invert

#### tee: copies standard input to standard output, making a copy in file
used when you need to store intermediate output  

Command | Description
:----: | ----
ls | tee abc.txt | something_else

#### jq: JSON Command Line Tool
not standard, and yes there could be a large debate of this command, but very useful in today’s world of API’s  

Command | Description
:----: | ----
curl http://www.espn.com/abc.json | jq '.' | no, espn does not have an API

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

man hier | get an explanation of the system directory structure

## [Advanced](#advanced)


## [Command Cocktails](#command_cocktails)
Command | Description
:-----: | -----
grep . *.txt |
diff <(ls) <(ls) | compare two commands, rather than having to write to a file first
read var1 | take stdin to variable
wc < abc.txt | use this instead of cat abc.txt \| wc, saves a new process, important only for big files; don't pipe a cat
host | get ip of domain
(head -5; tail -5) < data | explore first 5 and last 5 lines
head -3 data* \| cat | list out first three lines of all files
time read | simple stopwatch

## [Sysadmin Basics](#sysadmin_basics)

free | free space


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
n (forward) N (backward)
gg | top of file
G | bottom of file
25G | Goto line
% | go to matching parentheses
3igoESC twice | insert go 3 times
/| next/previous of the word that you are at
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

