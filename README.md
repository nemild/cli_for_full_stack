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
* There’s a mix of BSD and Ubuntu (many people use Mac for dev and a Ubuntu-type instance for the server) - this probably needs to be better called out; this is focused on bash
* This doc includes other basics of the CLI (keyboard shortcuts and very basic vim) with the assumption that this helps people get around CLIs (e.g., when ssh-ing into a server)

## Other Resources
* [Bro Pages](http://bropages.org/): Lists popular options for many of the most used commands, crowdsourced and upvoted (used to augment several commands below)
* [tldr pages](https://github.com/tldr-pages/tldr)
* [Command Line Fu](http://www.commandlinefu.com/commands/browse/sort-by-votes): Lists popular commands, use link to see top voted over all time
* [Sobel's Guide to Linux Command's Editors, and Shell Programming](http://www.amazon.com/Practical-Guide-Commands-Editors-Programming/dp/013308504X/ref%3Dsr_1_1): One of the more popular Linux books, includes examples and many basics (perl, bash programming, python) - fairly distribution agnostic
* [Digital Ocean Tutorials](https://www.digitalocean.com/community/tutorials/)

##### Authors
* Nemil Dalal (nemild at gmail)
* Evren Cakir (code at evrencakir)
* *[If you spend more than 15 mins on this doc, add your name here]*

## Table of Contents
<a href="#keyboard_shortcuts">Keyboard Shortcuts</a>  
<a href="#getting_around_the_command_line">Getting around the command line</a>  
<a href="#basics">Basics</a>  
<a href="#advanced">Advanced</a>  
<a href="#command_cocktails">Command Cocktails</a>  

<a href="#sysadmin_basics">Sysadmin Basics</a>  
<a href="#setup">Setup</a>  

## [Keyboard Shortcuts](#keyboard_shortcuts)
Keyboard Shortcut | Description
:-------: | ---------
*Ctrl-C* | terminate program, TERM signal
*Ctrl-\\* | QUIT signal
*Ctrl-A* | go to start of line
*Ctrl-E* | go to end of line
*Ctrl-W* | cut the previous word
*Ctrl -* | undo
*Ctrl-U* | clears the line before the cursor position.
*Ctrl-K* | Kill the line after the cursor
*Ctrl-U/Ctrl-Y* | Cut line, write text and then paste line after new text
*Ctrl-XX* | Move between start of line and current cursor position
*Ctrl-X/Ctrl-E* | use text editor to input command, used for long or multiline commands, can also use `fc`
*Ctrl-L* | clear screen
*Ctrl-D* | terminate program (send EOF)
*Ctrl-Z* | suspend program, `fg` to return
*Ctrl-R* | search command history, start typing command, ctrl-r again to cycle through other matches in reverse chronological order
*Ctrl-G* | Escape from search command mode (may be just easier to use Ctrl-C)
*ESC-.* | past last argument of previous command
*Ctrl-B/Ctrl-F* | Page up and down
*Ctrl-D/Ctrl-U* | Half page up and down
*Ctrl-S/Ctrl-Q* | stop output to screeen (for verbose commands) / resume output to screen
*Ctrl-T* | swap previous two characters (used for misspellings)
*Meta-T* | swap previous two words
*ESC-BACKSPACE* | delete previous word
*ESC-d* | delete next word

## [Getting around the command line](#getting_around_the_command_line)
#### Other Basics
* Three streams are stdin, stdout, and stderr
* find . 2> # the 2> pipes out stderr only, can use &> to pipe out both stdout and stderr
* pipe both standard error and stdout is |&

#### Previous Commands or Arguments
```bash
history # see list of previous typed commands
history -c # clear list of previous typed commands
history | grep command # list out previous calls with command, then do !123 to run where 123 is the number shown for the command
!! # run previous command
!string # run most recent command starting with string
!string:p # print most recent command starting with string
!?string? # run most recent command with this substring
!105 # run command number 105 from history
!-2: Run command 2 before the current command in history

^abc^def^ # replace first argument with second argument in previous command (only for first occurrence)
!!:gs/foo/bar # run last command replacing all foo with bar

!:$ # last argument of previous command (also !$)
!:0 # first argument of previous command (and number can be incremented for other args)
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

#### Other basics
Command | Description
:-----: | -----
which command | determine which command path is used to run command, used when you may have multiple binaries (looks through directories in your search path in order, and lists the first found)
whereis command | looks through list of standard directories independent of search path, displays all files found (not just first)
env | print system environment variables
export PATH=$HOME/bin:$PATH | add bin path to the existing path

## [Basics](#basics)

##### Commonly used flags across Linux commands
Flag | Description
:---: | ---
-v | verbose or invert
--version | show version
-i | ignore case
-w | ignore whitespace
-u | user
-h | pretty format the size (from bytes to mb/kb/etc), if using this when sorting, use sort -h (only non-Mac)
--color | use color
-r | reverse listing (descending order) or recursive
ls -- -l | the double hyphen indicates that everything afterward should be considered an argument, not an option (the 'l' is not considered a command line flag, but an argument)

#### cd: Change Directory
```bash
cd [ANY DIRECTORY] # base usage
cd # go to home
cd ~ # go to home
cd - # go to previous directory
cd .. # go up one directory
(cd ~/asd && ls) # execute command in subshell, at end you are back in your current directory
```

#### pushd/popd: reference your working directory as a stack
```bash
pushd /home/test/ # push directory to stack
pushd /home/test/show/ # push directory to stack
pushd /home/test/doc/ # push directory to stack
dirs -v # list all directories in stack with index number
popd # pop the most recent directory from stack and cd into it
popd +1 # pop a particular directory from the list of directories (without going to that directory)
```

#### mkdir: Make Directory
```bash
mkdir dirname # base usage
mkdir -p dirname1/dirname2 # make all nested directories that don’t exit
rmdir lets you remove an empty directory
mkdir -p foo/{bar,baz} # create both bar and baz subdirectories
```

#### ls: List directory contents
```bash
ls -l # provide information (ownership, permissions, file size, modification date)
ls -h # make disk usage info pretty (kb, mb, etc.) - not bytes
ls -r # reverses order, now ascending rather than descending
ls -t # sort by time modified, most recent at top by default (descending)
ls -G # colorize
ls -a # show hidden files/directories (all files/directories beginning with a dot)
ls -S # sort files by size
ls -F #  Display a slash (`/') immediately after each pathname that is a directory, an asterisk (`*') after each that is executable, an at sign (`@') after each symbolic link, an equals sign (`=') after each socket, a percent sign (`%') after each whiteout, and a vertical bar (`|') after each that is a FIFO
ls -d */ # directories only
ls -p # ack -v # files only
ls -ld dir/ # see details of a directory, not listing of items in a directory (used to see permissions)
ls -i # print inode
ls | wc -l # number of items in directory (including subdirectories)
ls -R . # recursively list the files and directories
ls | grep -v *.rb # filter out unwanted results, like .rb files
ls -lart # list files in the current dir by the last time they were modified. (-l = long, -a = all, -r = reverse, -t = order by mod time)
```

Popular usages  
```bash
ls -lah
ls -ltr # sort by time, with latest at bottom
ls -larthG # same as above plus show hidden, human readable file size, colorize
```

#### man: Manual
```bash
man command # shows manual page for command typically using less
man -k keyword # do a keyword search for manpages containing a search string

# The following are popular commands in man
Ctrl-B/Ctrl-F # page up and down
/searchterm # to search forward, can use standard vim commands including n and N to go forward and backward to searchterms, use this to search for flags ('-n')
?searchterm # search backward
h # display help, keyboard shortcuts
man ascii # ascii table
man hier # explanation of file systen
man 2 ls # go to section 2 of ls, search for what each section number means
# Most users find their info in sections 1 (User commands), 6(Games), and 7(Miscellaneous)
# use with whatis (complete word matches) and apropos/man -k (partial word matches), to determine what commands are available
# can also use info or help
```

#### cat: Print a file to the screen
```bash
cat abc.txt def.txt # can use on multiple files
cat abc.txt def.txt > new.txt # concatenate multiple files in order into a single file
cat *.txt # output all files matching *.txt

cat > file.txt # Create file, edit, then CTRL-D to save and exit

cat -n # number lines starting at 1, can also call nl -b a
cat -b # number non-blank lines, can also call nl -b t
cat -T # Show tabs in a file
Note: Don’t pipe a cat ('cat hello | dosomething'), instead 'dosomething < abc.txt'

# backwards cat is tac
```

#### less: Print a file with pagination
used to print files with pagination, can use standard keyboard shortcuts like page up and down, plus some vim commands like search (/ and ?, n and N). q to quit, arrow keys (and vim navigation keys) to move up and down, switch between files

```bash
less filename.txt # print file with pagination
less filename.txt filename2.txt # open multiple files, can use :n and :p to move back and forth
less +F # use instead of tail -f, can Ctrl-C, /searchterm, and then type F to have search terms highlighted
less -r # retain color codes, e.g., use with color options on the command you are piping to less
less -S # chop lines that go past screen end (rather than wrapping)
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
tails -n +10 file.txt # show last lines of file starting with 10th line

tail -f # don't exit, but keep outputting data as appended (used especially in viewing logs), see the less +F command for a potential replacement
tail -F # same as -f except if the file is replaced or truncated, follow the new file that is now pointed to by the filename
tail -f "my_server.log" | grep "my process" # Filter lines containing a match for "my process" from a text stream
tail -f /var/log/*.log # tail all log files 
tail -f /var/log/syslog -f /var/log/auth.log # multiple files
```

#### echo: Write arguments to the standard output
```bash
echo "line1\nline2" >> .bash_profile # append string to end of .bash_profile, quick way to edit
echo "line1\nline2" > output.txt # quick way to create file with text, compare to touch and then nano/vim
echo -n "Hello" # this line is not followed by a newline as is standard
echo -e "Hello\nWorld" # interpret backslash escape sequences like newline ('\n') and tab ('\t')
echo -e "\nalias ..='cd..'" >> .bash_profile # quickly append text to the end of a file

# multiline
echo "This is the first line
This is the second line"

echo $var # print shell variable
echo $(pwd) >> log # way to run a command and append to file
echo "ls -l" | at midnight # execute command at a certain time
echo {a..z} # print a through z, can be used for numbers as well
```

#### ln: Make link
Used primarily for symbolic or soft link
```bash
ln -s /a.out b.out # create symbolic link, a pointer to the original file - use to create a pointer rather than a deep copy, ls -l will show what a symbolic link points to, make sure to use absolute link path for the source file/dir
ln -sb # backup of existing files if overwriting
ln -sf # if target file link exists, unlink so that the new link may occur
```

#### grep: File Pattern Searcher
For most programming needs, it is preferable to use [ag](https://github.com/ggreer/the_silver_searcher) or [ack](http://beyondgrep.com/)  

```bash
grep 'substring' --color file1.txt file2.txt # search for substring can be used with multiple files
grep 'substring' -riI --color . # recursive (-R is the same), ignore case, ignore binary file matches, highlight match in color, a common usage

# The basics (substring and path follow the flag)
grep -i # case insensitive
grep -v # invert
grep -c # incidence count
grep -x # exact match
grep -n # show line numbers
grep -w # search for whole words
grep -l # list filenames with matching content
grep -I # ignore binary file matches
grep -m n # show only n number of matches in each file

# Show context
grep -A # 3 lines after
grep -B # 3 lines before
grep -C # 3 lines before and after

# include exclude files and/or folders
grep --include="*.js"
grep --exclude="*.js"
grep --exclude-dir="./tmp"
grep --include-dir="./directory_to_scan"

# Regular expressions
grep -E # interpret pattern as a regular expression, can also use egrep
grep -E -o # print only matching part of the line (-o option is used often with -E option)

# For gzipped files
zgrep –i substring /var/log/syslog.2.gz

# No Regular Expressions (faster grep)
grep -F # or fgrep

# Getting pattern from a file
grep -f pattern_file file

# Popular usages
grep -iIrn computesSum * # Case insensitive search through all of the files recusively starting from my current directory for the string "computesSum" but does not search binary files.  When a match is found, print the line number it was found on
grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' input.txt > output.txt # grep for IP address
grep -r --include="*.py" "pattern" /path/to/dir # grep recursively through files of a certain type, such as .py files
grep -m 1 "needle" *.txt # show only the first match in each file
```

#### ack (and ag): grep-like tool
Optimized for programmers searching source codel; ignores .git folder, minified files, etc.  

Many ack flags are similar or the same as grep; most programmers using git should use [ag](https://github.com/ggreer/the_silver_searcher) which is faster than ack and has a few other benefits (respecting .gitignore, etc.)  

**ack**
```bash
ack 'substring' # search for substring
ack --ruby 'substring' # find files in a specific language
ack -w 'substring' # whole word search
ack -l 'substring' # list filenames with matching content
ack -C 3 'substring' # show context around match

ack -cl # list filenames with matches, along with incidence
ack -ch # list number of total matches
ack --python # restrict search to a language
ack -f . # list all files that will be searched
ack -g substring # look for files in subdirectories that has substring in its path
```
  
**ag**
```bash
# defaults to ignoring binary files plus those in .gitignore, .hgignore, .agignore and svn:ignore; default folder depth is 25

ag -i # ignore case
ag foo -G bar # find 'foo' in filenames matching bar

# File types
ag --list-file-types # list file types that can be specified
ag --html # search only html files, with extensions as listed in previous command

# restrict to files based on regex
ag -g pattern # print filenames matching pattern
ag -G pattern substring # only searches filenames matching pattern

# caveat for logs/piping: defaults to printing only the first 10k matches per file, and 2k characters per line
```

#### find: Walk a file hierarchy (search for files etc. matching certain criteria, can also be used with xargs/parallel )
```bash
find . -name "hello.txt" # standard usage
find / -name "ice.log" # search filesystem root for files named ice.log 
find . -name “abc” 2>/dev/null # output to dev null when doing global search, and don't want to discard errors (e.g., file is not accessible because not a super user)
-iname "abc.txt" OR -iname "*.txt" # search for case insensitive
-mtime -7 # modified time is within seven days previous to today, can also use 7s, 7m, 7h, 7d, 7w to specify seconds/minutes/hours/weeks
-ctime -7 # created time is within 7 days of today
-print0 # used with xargs -0 option, can pipe find results to xargs
-type l/f/d # search for a type of item, f = file, l = symbolic link, d = directory
-not # invert all filtering criteria
find abc/ # lists everything recursively in directory
find -inum 16187430 # inode number 
find -maxdepth 1 # only list in current directory, not subdirectories
find -perm 444 # search based on permissions

# Popular usages
find . | wc -l # number of files in directory and subdirectories
find . -name "*.foo" -exec tail {} \; # output the last 10 lines in each .foo file in directory and subdirectories
find . -iname '*foo*' # find all files that contain foo in their name
find . -type d -empty -delete # find and remove empty directories
find . -type f -empty -delete # find and remove empty files
find . -type d -ctime -5 # find all directories created in the last 5 days
find ./ -not -type d -printf "%T+ %p\n" | sort | tail -1 # find last modified file in currenct directory
find -L . -type l # find broken sym links
find ~/chat -name *.txt | xargs grep 'substring' # grep for term in each matched file
```

#### jobs: (and fg and bg)
Job is a program you interactively start that is not a daemon. Jobs in background that required input are automatically stopped.
```bash
jobs # list out jobs, states are Running, Stopped; + is most recent job that is still running, - is second most recent
sleep 10000 & # start job in background
sleep 100000 # start job in foreground, then Ctrl-Z to suspend, each new job is higher number than others currently running
bg %2 # background the suspended job (assuming the number is 2), bg on its own will background the highest numbered job
fg %1 # bring job 1 to foreground, also can do %1, fg on its own will foreground the most recent/highest numbered job
fg %find # search for starting of string
fg %?ace # search for substring
kill %2 # kill job number 2
```

#### nohup: No hangup
Keep command running when logged out

```bash
nohup command | run command, ignoring any hangup signals (used when starting a process on a server which you want to keep running when you logout)
```

#### tar: Archive file or path
Create a single file from multiple files and/or subdirectories (old tape archive)
```bash
tar -cvf . ~/abc.tar # compress, verbose, compress to filename provided
tar -tvf # get info
-xvf # extract, verbose, extract filename provided
-xvzf # extract, verbose, uncompress gzip, extract filename provided
-cvzf # gzip as well as archiving
-cvjf # bzip2 as well as archiving
-cvJf # xz as well as archiving
```

#### gzip: Compress
xz and bzip2 are slower to compress but often much smaller (below), but gzip is still commonly used
```bash
gzip -l abc.gz # info on compressed file (compression ratio)
-d # decompress
```

#### bzip2 and xz
**bzip**
Similar options to gzip (though not exact). xz is slower and requires more memory, but leads to smaller output files.

```bash
bzip2 abc.tar out.bzip # compress
bzip2 -d out.bzip # decompress
bzip2 -r archive_name.bzip folder_to_compress

xz abc.tar out.xz # compress
xz -d out.xz # decompress
xz -l out.xz # list
```

#### zip – Cross platform zip (Mac OS X) 
```bash
zip -r archive_name.zip folder_to_compress #compress
unzip archive_name.zip #extract
zip -r -X archive_name.zip folder_to_compress #make a zip and exclude invisible Mac resource files such as “_MACOSX” or “._Filename” and .ds store files
```

#### sort
```bash
sort abc.txt def.txt # ascending, can do multiple files
sort /path/to/directory/*.csv # sort all files

sort -r # descending, z to a, 10000 to 1
sort -n # sort according to numerical value on line (even if there is other text on the line)
sort -R # randomize, GNU sort
sort -f # ignore case
sort -u # show max 1 of each unique element
sort -k 2 # start at key 2 (used for seprated columns, like the history command)
sort -h # sort by human readable size (use with -h on another command, like du -h), GNU sort
```

#### uniq
Note: only compares adjacent lines for equality, so typically need to sort in advance  

```bash
uniq -c # give incidence of line, will show repeated only once
uniq -d # only output duplicated items, single time
uniq -u # only output lines not repeated (different from sort -u)
uniq -i # case insensitive

uniq -cd # Show count of lines that have duplicates
```

#### wc: Word Count
```bash
# by default shows num lines, words, characters (in that order)
wc -w # word count only
wc -l # line count only
wc -c # bytes
wc -m # characters

wc -L # longest line length, used often for style in code, where 80 lines max may be a desired norm in a team

find path/to/dir | xargs wc -l # count the number of lines of all files in a directory
```

#### ssh: Open SSH client
set the ~/.ssh/config file to make life easier on client side for sshing into commonly used servers  
~/.ssh/authorized_keys  # list of public keys that are authorized to login to this server  

```bash
ssh -i ~/.ec2/abc.pem hello@100.1.24.33 # ssh into server with public key at path
ssh -i ~/.ec2/abc.pem hello@100.1.24.33 "command_to_run" # ssh into server with public key at path, run command_to_run, and disconnect
ssh -p 1000 remote_host # ssh to port 1000 on remote
ssh hostname "cd abc && ls -lah" # ssh then run command
ssh -D 1234 hostname # use hostname as a SOCKS proxy on localhost:1234
ssh -f -N -R 8888:example.com:80 username@remote_host

# Can suspend a session by pressing 1. Enter 2. ~ 3. CTRL-Z
# Tilde (~) lets you send commands to SSH

# Setup
ssh-keygen # to generate private and public key ~/.ssh/id_rsa and ~/.ssh/id_rsa.pub
ssh-keygen -p # change or remove passphrase
ssh-copy-id username@remote_host # automated way of copying public key to remote server's authorized_keys file
sudo nano /etc/ssh/sshd_config # Uncomment and ensure 'PasswordAuthentication no' and 'PermitRootLogin no' to prevent login by password and root login, can also change default port to reduce risk of intrusion; will need to restart afterward: sudo service ssh restart
```

#### sftp: Secure FTP
```bash
sftp ftp.example.com
sftp user@ftp.example.com

# upload/downlod
put file.txt
get file.txt

# batch upload/download
mput *.txt
mget *.txt

bin # binary mode

# commands on local file system can be run by appending l in front
lpwd
lls
lcd dir
lmkdir dir
```

#### mv: Move file from src to dest
```bash
mv src_file_or_directory dest_file_or_directory # base usage
mv srcfile srcfile srcfile destdirectory # can move multiple sources at once
mv hello.{txt,old} # quick rename
```

#### rename: Batch rename
```bash
rename 's/\.html$/\.php/' *.html # rename html file extensions to php
-n # add this flag to list what would change without changing
-v # add this flag to show changes made
```

#### cp: Copy a file from src to dest
```bash
cp src dest # base usage
cp -r src dest # recursive
```

#### rm: Remove file
```bash
rm abc.txt # base usage, remove file
rm -rf abc # recursive deletion (including subdirectories), f is force delete, use with caution
rm -i # prompt before deleting
rm !(*.foo\|*.bar\|*.baz) # delete everything but

# can also use srm for secure removal
srm -m /path/to/file # 7 pas
srm /path/to/file # 35 pass
```

#### diff: Compare two files
first lines in output refer to source lines, then a letter (a = add/+, c = change/!, d = delete/-), and then destination lines
```bash
-c # context mode
-u # unified mode
-y # side by side

-b # ignore things that change the whitespace only
-b # ignore blank lines
-w # ignore all space
-i # ignore case

-a # treat file as text
-C 3 # output num of copied context, 3 lines is default
-q # show only that the files differ, useful for scripts

diff -r DIRECTORY1 DIRECTORY2 # show the difference between two directories
diff -rq DIRECTORY1 DIRECTORY2 # show the difference between two directories, only showing the names of files that differ

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

#### tmux: Terminal Multiplexer
Enables a number of terminals to be accessed and controlled from a single terminal; great for multiple panes/windows/sessions (windowing) and persistent state on remote servers  

Hierarchy:  
Sessions (Overall theme, like work or sysadmin) > Windows (Projects within theme) > Panes (Views within a project, seen on a single screen)  

Highly encouraged to name sessions and windows  

Note:
* CTRL-b is the prefix key required before pressing a key (can be changed - e.g., for ex-users of screen); many commands allow you to use this as well as an actual command
* May consider remapping CAPSLOCK to CTRL to make life easier

**The Basics**
```bash
# ---------------------------------------------
# SESSIONS
# ---------------------------------------------
tmux new -s session_name # start and attach
tmux a # attach to first available session
tmux a -t session_name # attaches to an existing session
tmux ls # list sessions
tmux detach # CTRL-B d

tmux rename-session -t <old> <new> 
tmux kill-session -t <session>  

# CTRL-b ? # List all keybindings
# CTRL-b : # command prompt where you can execute any command tmux supports
```

**Main Keyboard Shortcuts**  
Put the prefix key before keys listed below (default: CTRL-B)

| Command<br/>(Key) | Session | Window | Pane |
|:------:|:-------:|:-------:|:-----:|
|Create|:new -s SESSION_NAME <br />|New: c <br /> *Pane to Window:* !| *Horizontal Split:* % <br />*Vertical Split:* "|
|Rename|$|,||
|List|`tmux ls`<br />s SESSION_NAME|w ||
|Search||f SEARCH_TERM||
|Close / Kill|:kill-session -t SESSION_NAME|&|x|
|Previous / Next | ( / ) | p / n | Arrow Key (selects pane in specified direction) |
|Last||l|;|
|Select / Move to index||NUMBER|q NUMBER|
|Swap|| :swap-window -s 3 -t 1 (Keyboard: . NUMBER) | { } |
    
**Other**  
```bash
# ---------------------------------------------
# COPY/PASTE (with emacs bindings)
# ---------------------------------------------
CTRL-b [ # enter copy mode
     * SPACE to start copying text at cursor
     * move cursor to end of desired text
     * ENTER to copy selected text

CTRL-b ] # paste selected text

# ---------------------------------------------
# OTHER
# ---------------------------------------------
tmux info # list out every session, window, pane, etc.
tmux list-commands # list out every tmux command and its arguments
tmux list-keys # lists out all bound keys, and relevant tmux command
tmux source-file ~/.tmux.conf # reload the tmux config file. tmux default config file is in ~/.tmux.conf

# CTRL-b t # show time in current pane

# Kill all tmux windows
killall tmux

# Use this to start tmux on iTerm 2
# will use iTerm 2 interface for tmux, but will let you restore environment when you need
tmux -CC
tmux -CC attach

# with credit:
# Josh Clayton: http://robots.thoughtbot.com/a-tmux-crash-course
# Cody: http://blog.hawkhost.com/2010/06/28/tmux-the-terminal-multiplexer/
```

#### cURL: Transfer a URL
Certain commands taken from [here](http://zaiste.net/2012/11/introduction_to_curl/)
```bash
# The Basics
curl http://www.example.com/index.html -o abc.html # output to file abc.html
curl -O http://www.example.com/index.html # output to the remote filename on local, in this case index.html

# Common Options
curl -X POST # can use the standard verbs, plus custom ones
curl -H "Accept: application/json" # set custom headers, can add multiple by using multiple flags # can also use --header
curl -d "birthyear=1905&press=%20OK%20"  http://www.example.com/when.cgi # send data to server in body, automatically POST, can also separate by separate -d arguments rather than an &
curl -d @invoice.pdf -X POST http://devnull-as-a-service.com/dev/null # post a file
curl -i http://www.example.com # show response body AND headers 

# Other
curl -v # verbose
curl --cacert file.pem
curl -d '{"user": {"name": "zaiste"}}' -H "Content-Type: application/json" http://server/
curl --data-urlencode
curl --form upload=@localfilename --form press=OK [URL] |
curl -u myusername:mypassword http://localhost # pass a username and password for server authentication

curl --head # head request only

curl ifconfig.me/ip # get IP
curl ifconfig.me/port # get port
```

#### wget: Network Downloader
```bash
# Basics
wget http://path.to/file
wget http://path.to/file1 http://path.to/file2

wget -O abc.html http://www.example.com/def.html # output to file
wget -i tmp.txt # download a list of URLs, file list has URLs listed one per line
wget -b # background download, with log file at: /wget/log.txt
wget -c http://www.example.com/abc.tar.bz2 # continue a previously interrupted download, typically make 'wget -c' an alias for wget so it is automatic


wget -pk http://path.to.the/page.html # To mirror a whole page locally
wget -mk http://site.tl/ # To mirror a whole site locally
wget http://www.myserver.com/files-{1..15}.tar.bz2 # To download files according to a pattern

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
Useful for cURL-ing APIs and json files, non-standard so install with package manager of choice

```bash
curl http://www.example.com/abc.json | jq '.' # prettify json
curl http://www.example.com/abc.json | jq '.[0]' # restrict output to first element in array
curl http://www.example.com/abc.json | jq '.abc.def' # restrict output to "def" key within "abc" key
jq 'length' < abc.json # number of elements in JSON array
jq '.[0] | {message: .commit.message, name: .commit.committer.name}' < abc.json # create two new keys with the associated data from the input (can nest)

-S # sort keys in alphabetical order
# Just the basics, see docs for more details: http://stedolan.github.io/jq/manual/
```

#### convert: Image conversion
Image conversion using ImageMagick - useful especially for resizing and converting formats.
```bash
convert src.jpg dest.png # convert from one format to another
convert image.png -resize 50% out.png
convert a.png b.png c.png +append out.png
```

#### at: Schedule command for later execution
```bash
command at {time}

# {time} can be either
# now | midnight | noon | teatime (4pm)
# HH:MM
# now + N {minutes | hours | days | weeks}
# MM/DD/YY

atq # To list pending jobs
atrm {id} # To remove a job (use id from atq)

```

#### mount: Mount file systems

```bash
mount # show all mounted file systems
mount | column -t

mount /dev/sdb1 /u01 # mount device to directory
```

#### Other commands
Command | Description
:----: | ----
pwd | print working directory
touch abc.txt def.txt | create empty file or files
date | date and time
date +%s / date -r 1231006505 | to/from unix epoch time
date +"%Y%m%d_%H%M%S" | print out date in format for affixing to filenames
cal / cal 3 1973 / cal -m 3 / cal -y 2013 | calendar of current month
dirname/basename | basename will strip suffix provided as second argument
whois hostname | 
pidof | 
mcookie | generate pseudo random string
whatis command | short description of command
apropos command | find commands like this
pbcopy / pbpaste | macintosh, pipe to/from these for clipboard, see alias section for Linux
units | convert between units
hostname | displays the system name
read var1 | set variable to stdin
yes | print y forever, used if you want dummy text, often used with head to restrict to a certain number of lines, can also tack on a prefix/suffix to make each line unique
script | record everything typed in a text file
file abc.txt | get info on file type
clear | clear screen

## [Advanced](#advanced)

#### ifconfig: Configure network interface
```bash
ifconfig -a # view all network interfaces
ifconfig eth0 # show a single interface
ifconfig eth0 up # turn on
ifconfig eth down # turn off
ifconfig eth0 192.168.2.2 # assign IP address to interface, may need sudo privileges

ifconfig eth0 promisc # turn on promiscuous mode
ifconfig eth0 -promisc # turn off promiscuous mode
```

#### gcc
```bash
gcc def.c # compiles, assembles and links a program, executable is in a.out
gcc -c abc.cpp # compile cpp
gcc abc.o def.o -o def.out # link and create binary with filename, -o is output
gcc -Iabc/def/ # look for include files in directory before looking in standard locations (can use mutliple of these)
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
# Others: -r, copy recursively, -h human readable numbers
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
[General Tutorial] (https://www.digitalocean.com/community/tutorials/how-to-use-the-awk-language-to-manipulate-text-in-linux)
[awk one liners](http://www.pement.org/awk/awk1line.txt)

```bash
awk '/search_pattern/ { action_to_take_on_matches; another_action; }' file_to_parse # standard structure, print is default action if nothing specified
awk '/search_pattern/ { print $1; } /etc/fstab # print column 1
awk -F ',' '{print $3}' filename # print 3rd column in CSV file
awk '{s+=$1} END {print s}' filename # Sum the values in the first column and print the total
```

#### tr: Translate Characters (replace a character with another)
```bash
tr ' ' '\_' < abc.txt # replace spaces with underscore, and print to stdout
echo abcdef | tr 'abcdef' 'xyzabc' # each single character in the first string is translated to the corresponding character in the second string
tr '[:lower:]' '[:upper:]'
tr "A-Za-z" "a-zA-Z" # reverse case
tr "a-z" "A-Z" # make lowercase uppercase
tr -d '\r' # delete character, useful when moving txt files between Windows and Linux
tr -dc 'a-z A-Z \t \n '\32'' # invert delete
tr -s '\n' # make multiple instance just a single instance
tr '[:upper:]' '[:lower:]' # upper to lower case
```

#### time
`time ls # run ls and profile time`  
wall/real time - can be less than some of system + user time, given parallelism  
system time (system calls) vs user time (time spent running user requested command)  

#### sleep
```bash
sleep 5; echo "done"  # sleep for 5 seconds then proceed
sleep 5s # sleep for 5 seconds
sleep 5m # sleep for 5 minutes
sleep 1d 5h 5m 10s # can create a list, Linux only
# useful in loops to delay
```

#### sed: Stream Editor
Primarily used for substitution (leading 's' below)
```bash
sed 's/day/night' abc.txt def.txt # replace first 'day' with 'night' per line, can use with multiple files
sed -e 's/day/night' -e 's/something/else/g' abc.txt # -e lets you specify separate commands
sed 's/day/night/;s/something/else/g' abc.txt # same as above
sed -i # inplace editing, replaces file
sed -i.bak # inplace editing, replaces file, and saves backup file with given extension

sed 'G' abc.txt # enters a blank line after each line
sed '=' abc.txt # add line number before each line

sed '/hello/s/world/& !/' hello.txt # replace only on lines that match a certain pattern
sed '/^$/d' file.txt # delete all empty lines

sed '5s/day/night' abc.txt # substitute only on line 5
sed '5!s/day/night' abc.txt # everywhere other than line 5

sed -E # use regular expression
sed -e s/[a-z]/(&)/ # & means use found section

# Common usages
sed '/^[[:space:]]*$/d' input.txt > output.txt # remove blank lines from a file
sed -e 's/<[^>]*>//g' index.html # strip HTML
```

#### seq: Sequence
```bash
seq 5 # print sequence of numbers starting at 1
seq 1 10 # print from 1 to 10
seq 1 3 10 # go from 1 to 10 in steps of 3
seq -s 'abc' 5 # prepend number onto string
```

#### jot: Generate Data
```bash
jot NUM LOW HIGH STEP
jot -b hello # repeat word
jot -w hello # use as start
jot -r # random
jot -s’:’ # print data with string as separator (not newline)
```

#### mktemp: Create Temp File
```bash
mktemp -t abc # use abc as template
-d # make directory instead of file
```

#### watch: Execute a program periodically, showing output fullscreen
```bash
watch -n 5 tail /var/log/messages # runs every 5 seconds after previous run completed
watch --precise -n 5 # tries to run 5 seconds from previous start
watch -d # highlight differences between successive updates
watch -e # exit if the return value is non-zero

watch -d ls -l # watch contents of a directory change
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
lsof -i TCP:22
lsof -i -sTCP:LISTEN # Find listening ports
lsof -iTCP:80 -sTCP:LISTEN
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

lsof -t /path/to/file | xargs kill -9 # only output the process PID, then pipe to kill

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
This command is often used in concert with [parallel](#parallel): split breaks up a file (like a data CSV), and then parallel lets you process each section concurrently
```bash
split -b 75m input.zip # cut into multiple files of 75m max
split -n 5 filename # split into 5 files
split -l 10 filename # split into files with each having 10 lines (except last)

cat `ls x*` > reassembled.zip
```

#### dtruss
TODO

#### openssl
```bash
openssl genrsa -out server.key 2048 # create a 2048 bit private key
openssl req -new -key server.key -out server.csr # create a Certificate Signing Request
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt # Sign certificate using a private key and CSR

openssl sha1 filename # get sha1 hash of filename
# can also use m5sum on file, to get md5 hash
md5sum file.txt

openssl dgst -sha256
openssl dgst -ripemd160
openssl s_client -connect www.domain.com:443 -showcerts # get certificate from a certain server
```

#### ping: Send request to network host
```bash
ping google.com # see if a site is up and you can connect to it, resolves domain to IP; does continuously till you terminate
ping -c 3 google.com # ping 3 times then quit
```

#### traceroute (Print route packets take to host) and mtr (Network Diagnostic)
```bash
traceroute google.com
traceroute -m 255 google.com # up to 255 hops away, default is 30
traceroute google.com 70 # change packet size to 70 bytes
traceroute -w 0.5 host # wait time for response
traceroute -q 5 host # queries per hop

# mtr combines ping and traceroute, main advantage is continous updating
mtr google.com
mtr --report google.com # sends 10 packets to each hop
```

#### host: DNS Lookup Utility
Get IP or other servers of domain
```bash
host -t mx google.com # MX servers
host -a google.com # all servers
```

#### nslookup: Query internet name serve
```bash
nslookup example.com
nslookup -query=mx example.com
nslookup -type=ns example.com
```

#### dig: DNS Lookup Utility
Testing for DNS info associated with a domain
```bash
dig example.com # defaults to A records
dig example.com MX # mail records
dig example.com ANY # all records
dig your_domain_name.com +short # IP that domain points to only

dig @8.8.8.8 hostname.com # pick another DNS server to query (8.8.8.8 is Google's)
```

[Start of More Advanced?]
#### ab: Apache Benchmarking
ab -n 100 -c 50 http://www.example.com/ # send 100 requests with a concurency of 50 requests to an URL
ab -t 30 -c 50 URL http://www.example.com/ # send requests during 30 seconds with a concurency of 50 requests to an URL

#### tcpdump: Dump traffic on a network
(Note: This needs to be culled)
```bash
# Basics
tcpdump -i eth0 # capture traffic of a specific interface
tcpdump -A tcp # captura all traffic
tcpdump host 2.3.4.5 # capture all traffic to or from host (src, dest below restrict to one direction)
tcpdump -i eth0 src 192.168.1.1 dest 192.168.1.2 and port 80 # capture the traffic from a specific interface, source, destination and port

# for a quick primer on TCP look here: TODO
tcpdump -n # don't resolve names, keep IP addresses
-X # displays both hex and ASCII content within the packet
-S # absolute sequence numbers
-s 96 # only takes first 96 by default, can change, 0 is all
tcpdump -nS # Basic communication
tcpdump -nnvvXS
tcpdump -nnvvXSs 1514
tcpdump src 2.3.4.5
tcpdump dst 2.3.4.5
tcpdump port 3389
tcpdump portrange 21-23
-w capturefile # write to file
-r capturefile # read from file
```

#### netstat: Show network status

Basics:  
Ports 1 - 65,535
  - 1 - 1024 associated with Linux, must typically have root priveleges to assign to this range
  - 1024 - 49151 are registered ports, can be reserved by IANA
  - 49152 and 65535 are for private use

**Standard ports**  
Port Number | Service
:---:|:---:
20 | FTP data
21 | FTP control port
22 | SSH
23 | Telnet, insecure (don't use, use 22 instead)
25 | SMTP
80 | HTTP
110 | POP3
143 | IMAP
443 | HTTPS
587 | SMTP, message submission

```bash
netstat # print list of open sockets
sudo netstat -plunt # show port and listening socket associated with the service
netstat -a # list all ports
netstat -at # all TCP
netstat -au # all UDP

netstat -l # active listening

netstat -a # show statistics
netstat -r # display routing table information
netstat -c 1 # update at time
```

#### nmap: Host Discovery and Port Scanning

```bash
# uses SYN by default, Discovery mode of host (port 80 is used), then Scan
# need to use with sudo
nmap localhost
nmap 192.16.10.0
nmap scanme.nmap.org # test server
nmap -sn {{192.168.0.1/24}} # discover hosts in the 192.168.0.X area , without a port scan

nmap -O hostname # tells you Operating System information
nmap -PN remote_host # assume host is up
nmap -p 1000 remote_host # scan a specific port
nmap -p1-10000 # scan range of ports, default is 1663 ports
nmap -p22,23,10000-15000 192.168.10.0/24 # scan selected ports
nmap -sP abc.com # ping scan, sends ICMP echo and TCP ACK
nmap -sT remote_host # scan for TCP connection
nmap -n -PN -sT -sU -p- remote_host # scan for every TCP and UDP open port
nmap -sS remote_host # SYN scan

nmap -sF # FIN scan
nmap -sX # XMAS scan, Christmas Tree packet has all options on, used primarily in scans and DOS
nmap -sA # ACK scan
nmap -sN # NULL scan
nmap -oN # .nmap output, human readable
nmap -6 # scans ipv6
```

#### nc: reads and writes tcp or udp data
```bash
nc -l 80 # listen on port
nc 128.0.0.1 21 # connect to a certain port (then can write to it)
cat somefile.txt | nc -l 1000 # serve a file on port
nc 128.0.0.1 21 > somefile.txt # receive a file
```

#### icon: Convert from one encoding to another
```bash
iconv -f FROM_ENCODING -t TO_ENCODING input_file
iconv -l # list supported encodings
```

## [Command Cocktails](#command_cocktails)
```bash
grep . *.txt # prints all matched files in directory, and prepends filename to each line
diff <(ls) <(ls) # compare ouput of two commands, rather than having to write each to a file first
wc < abc.txt # use this instead of cat abc.txt \| wc, saves a new process, important only for big files; don't pipe a cat
(head -5; tail -5) < data # explore first 5 and last 5 lines
head -3 data* | cat # list out first three lines of all files
time read # simple stopwatch, Ctrl-C to stop
curl -sS https://getcomposer.org/installer | php # pull down a php file and run it with the php interpreter
```

## [Sysadmin Basics](#sysadmin_basics)
This section is for things related to user management, process management, filesystem, or the general system.

### Users, Groups and Permissions
#### useradd/userdel: Add/delete user
```bash
useradd some_username
userdel -r username # delete a user with home directory
userdel johndoe group | remove user from group
```

#### chown: Change ownership (chown)
```bash
chown bob file.txt
chown bob:new_group file.txt # change group and owner
chown -R user:group path/to/directory/ # apply recursively
chown --reference=file.txt secondfile.txt # transfer owner and group from reference to secondfile
chown -h bob path/to/symlink # change owner of symlink
```

#### chmod: Change mode
read is 4, write is 2, exec is 1, use sums  
user/owner (u), group (g), other/everyone else (o), all (a)  
For shell scripts, you need both r and x to run them, for binary only x
```bash
chmod 755 # rwx, rx, rx
chmod 644 # rw, r, r
chmod ugo+rwx abc.out # this commands shows everything at once, can also use minus (-)
chmod +x abc.out
chmod -x abc.out
chmod g+x abc.out
chmod go-rwx file.txt
chmod -R # apply recursively to all files in the subdirectory
chmod o=g file.txt # give others the same rights as the group

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

#### passwd: Change password
```bash
passwd # change password
passwd -d johndoe # disable password
passwd -a demo sudo # 
```

#### Other commands
Command | Description
:-----: | -----
whoami | Prints your username
who | list users on the system
w | users on system, show who is logged in and what they're doing (more info than who)
users | list of users who are currently logged in
finger username | info on user, including logins
last, last username | list of login history for a user or all users
groups | Outputs the groups to which your account belongs to.
  

### Processes and Jobs  

#### top: Display and update sorted information about processes
Linux and BSD flavors of top are very different
use htop, not top when possible  
```bash
top -U johndoe # see processes owned by johndoe
top -s 10 # 10 second delay between updates
top -n 10 # 10 iterations, then quit


# Keys
# M - sort processes by memory usage
# P - sort processes by processor usage
# k - kill current tagged process
# /substring - Search processes for substring

# O then letter from list - sort by variable
# z - show running process
# c - show absolute path
# k PID - kill pid
# r - renice
# h - help
# q - quit
```

#### ps: Process status
```bash
ps aux | grep process # standard usage, display info about all users processes; often used in conjunction with kill
ps axjf # tree view
ps aux | grep '[h]ttpd' # bracket first letter in grep to exclude the grep result
ps auxww # list all running processes including the full command string

ps -l # long option
ps -u abc,def # users
ps -f -p 123 # find by process id
ps -f -ppid 123 # find by parent process id
ps aux --sort pmem # sort by column
```

#### kill: Kill
```bash
# Basics:
# 1 - SIGHUP - Hang up signal. Programs can listen for this signal and act (or not act) upon it.
# 2 - SIGINT - Interrupt
# 3 - SIGQUIT
# 6 - SIGABRT
# 15 - SIGTERM - Termination Signal, default signal sent by kill
# 9 - SIGKILL - Kill Signal, immediate kill by the kernel

kill 2592 # kill PID 2592, send TERM signal when not specified
kill -9 2592 # non-ignorable kill, SIGKILL
kill %1 # kill job number 1 from 'jobs' list
kill -SIGTERM 2931 # can use any of the signals above
pkill -9 ping # same as kill -9 `pgrep ping`
kill -l # list all signal names
```

#### killall: Killall
```bash
killall command # kills all instances of command owned by user (root user kills every process)
```

#### pkill: kill all instances of a command
```bash
pkill -f httpd # kills all instances of httpd dameon owned by user
```

#### vmstat: Report virtual memory statistics
```bash
vmstat -a # info on memory, swap etc
vmstat -S M # outputs in megabytes 
vmstat 2 6 # update every two seconds, stop after 6 iterations
```

#### nice: execute a utility with an altered scheduling priority
-19/-20 are highest priority and 19/20 are lowest priority (lowest priority take minimal resources)
```bash
nice -n 12 myproc # starts  process "myproc" setting the "nice" value to 12
renice 17 -p 1134 # change the priority of job with pid 1134 to 17
```

### Disk

#### du: Disk Usage
```bash
# Basics
du /some/dir
du -sh /some/dir # disk usage of items in directory, but does not traverse (s = summarize) subdirectories
du -h --max-depth=1 # List human readable size of all sub folders from current location

du * | sort -r # display usage of each file and subdirectory in current directory starting with largest, can use with head to get the largest items at top
du -shc * # same as above, but adds a total at the end

du --max-depth=1 -b | sort -k1 -rn # largest files in directory, linux only, not Mac
```

#### df: Disk Free
```bash
df -h # disk free by volume, h is pretty format for size (mb, kb, etc)
df -lh # local file systems only
df -h --total # add total column, non-BSD

df -m # show in megabytes
df -h # show in gigabytes
```

#### free
```bash
free -mt # latter option uses space in megabytes with total
free -s 5 # refresh at 5 second intervals
```

### Package Management  
#### sudo
```bash
sudo command # become a root temporarily (to execute a command), prompts for password when first typed and then won't ask for password again for a 5 min window during that terminal session
sudo !! # run previous command as sudo
```

#### Package management - apt-get, brew, and install from source
```bash
sudo apt-get upgrade #upgrade packages
```

```bash
apt-get -y update && apt-get upgrade && reboot #update, upgrade all packages, and reboot
```

```bash
wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -;
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'; #add a repository
```

```bash
sudo apt-get install -y python-software-properties python g++ make # install all of the following packages without prompting for confirmation
sudo apt-get remove g++ # remove package
sudo apt-get update # update list of packages available
```

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

### Hierarchy (Key locations/files to know)
```bash
cat /etc/passwd # list of all users
cat /etc/group # list of all groups on system
cat /etc/shadow # list of all hashed passwords

# Other key folders
# /bin - keep basic commands, minimal needed to boot
# /dev - devices
# /root - home directory of root user
# /usr - stores all non-essential programs
# /etc - main configuration directory
# /vars - system logs and backups

man hier # get an explanation of the system directory structure
```

### Other  
#### alias: Create a command alias
```bash
alias m='less' # create alias m to call less command
unalias m # remove alias
```

#### export: export shell variables
```bash
export EDITOR=/usr/bin/vim # set a new EDITOR variable:
```

```bash
VAR=value # alternative syntax
export VAR
```

Add export statements to ~/.bash_profile or ~/.profile or /etc/profile file to export variables permanently
```bash
$ vi ~/.bash_profile
```

#### source: executes the content of the file passed as argument, in the current shell. It has a synonym in '.' (period)
```bash
source ~/.aliases # executes ~/.aliases script 
```

#### shutdown: Shutdown System
```
shutdown -r now
shutdown now
shutdown -h +10
```

#### service
```bash
service ssh start
service ssh restart
service ssh stop
```

#### dd: convert and copy a file
```bash
# Read from /dev/random 2*512 Bytes and put it into /tmp/abc.txt
dd if=/dev/random of=/tmp/abc.txt count=512 bs=2
```

#### Other commands
Command | Description
:-----: | -----
uptime | time system has been up
uname -a | get all system information
echo $0 | determine which shell (e.g., bash)
hash, hash -r | table of where commands can be found based on usage, -r empties table when you may have changed the path