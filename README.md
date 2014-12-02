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
*Ctrl-X/Ctrl-E* | use text editor to input command, used for long or ltiline co*mmands
*Ctrl-L* | clear screen
*Ctrl-D* | terminate program
*Ctrl-Z* | suspend program
*Ctrl-R* | search command history
*ESC-.* | past last argument of previous command
*Ctrl-K* | delete from here to end of line
*Ctrl-B/Ctrl-F* | Page up and down
*Ctrl-D/Ctrl-U* | Half page up and down

## [Getting around the command line](#getting_around_the_command_line)


## [Basics](#basics)


## [Advanced](#advanced)


## [Command Cocktails](#command_cocktails)


## [Sysadmin Basics](#sysadmin_basics)


## [vim basics](#vim_basics)
DISCLAIMER: This is just to help someone get the basics of Vim, not for regular users  

### General Notes


### CLI Commands
Command | Description
:------: | -----
vimtutor | you can type this at the command line to get a Vim textual tutorial
vim filename.txt +25 | open file and go to line 25
vim filename.txt +/abc | open file and go to first occurrence of substring

### Vim Commands
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

### Commonly Used Aliases
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


## [Bash Scripting](#bash_scripting)

