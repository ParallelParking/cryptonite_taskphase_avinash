# PONDERING PATHS

Linux file system is a 'tree'
-> understanding this as per graph theory, there is a unique path to each individual directory, which is a node of the tree, and file, which must be leaf nodes

Nodes are separated by '/'
The root node is demarcated in path by a leading '/'

Invoking a program:
its path must be provided to the shell.
Providing a path beginning at root is known as an "absolute path"

NOTE THAT challenges on pwn.college live at /challenge

## CURRENT WORKING DIRECTORY
describes the directory the shell process is currently active in

This directory can be controlled via command "cd <fileloc>"

## RELATIVE PATHS
-> following on from my earlier comparatives to math, the idea that there must be a unique path to every node from the root node, coupled with the fact that all nodes of a tree must be root nodes of respective subtrees, means that there must be a UNIQUE path from every node to every other node

This means it is possible to construct "relative paths", i.e. the path is not specified as starting from the root. Here the path is interpreted as being relative from our current working directory (cwd)

i.e. our cwd is taken as root node of subtree and path is taken from there.

eg. path to /challenge/run relative to root node / is challenge/run

NOTE THAT ../ refers to parent directory in relative addressing.

Similar to above, ./dirname is equivalent to ././dirname, and dirname is equivalent to dirname/./.

## NAKED PATHS
Say we're in a situation wherein our cwd is same as our target program. Using logic from above, running it should be as simple as typing prog filename into terminal.

This, however, will not work. It's too risky to core system utilities, and shell will yield a command not found error

alternatively, we can simply call an implicit relative path to the file, i.e. ./filename and the program will run.

## HOME DIRECTORY
shell sessions at pwn.college begin with home directory as default.

~ is shell shorthand for the absolute path to the home directory

This is implicitly expanded ONLY when the path specified in a command has ~ as the leading character.

NOTE: cd with no arguments defaults to home dir.
