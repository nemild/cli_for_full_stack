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
* Nemil Dalal (nemild at gmail) - full stack dev
* *[Add your name if you spend more than an hour on this doc]*

## Table of Contents
<a href="#keyboard_shortcuts">Keyboard Shortcuts</a>
Getting around the command line
Basics
Advanced

Sysadmin Basics
Vim (Uber-)Basics
Command Cocktails
Setup
Bash Scripting

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
