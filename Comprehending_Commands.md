# COMPREHENDING COMMANDS

NOTE: flags are being written into here starting now. They are missing in modules 1 and 2.

NOTE: I'm aware that it was asked of me to "describe my thought process" in solving these problems, however every module I've done thus far has been essentially theoretical followed by a challenge composed of simply following instructions. Hence, I've included notes on everything I've **learned** over the course of this. The moment these challenges become less about *learning* and more about *solving* I will do the thought process thing in detail.

## 'cat'
The cat command reads out contents of file given as argument.
ODD BEHAVIOUR: providing multiple arguments to cat will simply cause it to print out concatenated outputs.

Finally, provided no arguments, cat will simply read terminal input and output it.

*CHALLENGE 1: pwn.college{kAVo-qxneT22cq29pA6p8PdYxBY.dFzN1QDLwEDN1czW}*

cat can also be used to read files via absolute path.

NOTE: the flag on pwn.college ALWAYS lives at absolute path /flag
Just generally, inaccessible via direct means.

*CHALLENGE 2: pwn.college{MfhyqehFsuhPcNcFzVFfWeW5oMV.dlTM5QDLwEDN1czW}*

*CHALLENGE 3: pwn.college{0FeqDeG3YzU8iBZB8o4MMkSEjUi.dBjM5QDLwEDN1czW}*

## 'grep'
The grep command derives its name from g/re/p, or "Globally (g) search for a Regular Expression (re) and print (p) it"

Hence the functionality of grep becomes obvious, it is a command that will search a given file for a substring and print all matching instances of it.

grep searchString /path/to/file

NOTE: all flags on pwn.college begin with the substring pwn.college.

*Challenge 4: pwn.college{s8G7GzdiGWpecAewBjEFQ3hGc-_.ddTM4QDLwEDN1czW}*

## 'ls'
ls stands for list. It will list all subdirectories present in the directory provided as argument. If no argument given, cwd is chosen as parent directory.

*Challenge 5: pwn.college{QYqjL4U9UCXlEwcycK9Clg4PKGO.dhjM4QDLwEDN1czW}*

## 'touch'
touch creates a file of arg name.

*Challenge 6: pwn.college{MaRSdI6o6sieL95CvAQXh7HjyAD.dBzM4QDLwEDN1czW}*

## 'rm'
rm stands for remove. It will remove the argument file.

*Challenge 7: pwn.college{4CDaAXwTwbzgi6sioNQpS7-gb7F.dZTOwUDLwEDN1czW}*

NOTE:
all directories and files with leading periods (eg. ".vscode") are called "hidden", and are thus ignored by the regular ls command. The use of this is to ease user experience in hiding system functionality dirs and files that they may not care about, and further preventing the user from messing with files that might risk system integrity.

However, to view these, it can be done via 'ls -a' 

NOTE: '-a' here is a "flag" to ls, that signifies strictly 'all' to be shown. There are several flags to linux commands.

*Challenge 8: pwn.college{s0Njp9ZTBm_r-49QBTkZTThFMoa.dBTN4QDLwEDN1czW}*

*Challenge 9: pwn.college{EaBpbnQtxCNZ0s_IUBO166sBVmT.dljM4QDLwEDN1czW}*

## 'mkdir'
mkdir stands for "make directory". Does exactly what it sounds like.

*Challenge 10: pwn.college{c9myGBE1tjIfc-umoWuRyLIjF41.dFzM4QDLwEDN1czW}*

## 'find'
"Finds" in a directory subject to certain criterion. The arguments find takes broadly describes 2 things: the criteria of search and the search location. Without the former, every file is matched, and without the latter the cwd is taken as implicit.

General syntax goes:
find searchloc criteria
eg. find /tmp -name avina
this will search directory /tmp for all files named avina.

*Challenge 11: pwn.college{I3wJ86ZZgove9DUf1P5FznTFhhZ.dJzM4QDLwEDN1czW}*

### challenge 11 details:
Finally one I can write about! Not much, though. It started with me using find with the root as search location against name 'flag' as criteria. A bunch of subdirs denied me access, then it was just a matter of brute forcing through all the accessible ones. used ls/cat on all mentioned until one file returned a flag.

## Linking files
https://www.geeksforgeeks.org/soft-hard-links-unixlinux/
https://www.redhat.com/sysadmin/inodes-linux-filesystem

There may be situations wherein two programs wish to access the same data but both believe this data is stored under different file names.

In order to resolve situations like this, "links" are used.

A fun way to think about links is that they are to files what pointers are to variables in C. a link tells us *where* a file is stored in the system as opposed to what that file actually contains.

This means multiple links can *refer* to the same original file data, even if only one copy of said file exists.

Types of links:
1. hard links
2. Soft links / symbolic links / symlinks

To understand hard links, we must understand what an "inode" is.

An inode is a unique identifier given to a piece of metadata on a given filesystem.
What does this mean? Recalling my earlier tree analogy to the linux filesystem (see: module 2), the filesystem of a linux system is the tree as a whole, and pieces of metadata are "nodes" of the tree.
In other words, an *inode* is an identifier to a node of the filesystem tree.

Now, there may be multiple copies of the same inode, but an inode can only refer to ONE node of the tree. Hence, it can be inferred that hard links are references to the same file via the mechanism that every hard link to a file has the same INODE to that file.

This is helpful in that regardless of where the node moves in the tree, or if the node's user facing name is changed, the inode remains the same and thus the hard link is maintained.

Further, a node can be directly accessed by its inode, so it is accurate to say every hard link to a node holds the actual data of that node.

HOWEVER the disadvantage is that inodes are filesystem specific, and hence system specific, and thus do not carry over across systems.

A hard link can be created via the command 'ln [original name] [link name]' where 'ln' stands for link.

Now what is a soft link?
We saw that hard link all referred to the same inode. The difference with symlinks is that each symlinks, regardless of if they point to the same node, will always have different inodes. This is because, instead of all symlinks holding the same inode as file to link to it, each symlink contains the location *of* our required file as data. In this sense, drawing upon our earlier C analogy, all hard links are single pointers pointing to the location of a file, whereas all symlinks are higher order pointers, holding the address of our required file rather than the actual data held in this file.

Due to this mechanical change in how linking works, symlinks have the benefit of being filesystem independent (no longer holding a specific inode means this works regardless of system).

Note that when deleting the original file, in hard linking each individual link could still show data, whereas in symlinking the links are left "dangling" to NULL and cannot display data.

hard link size was equal to data stored in original file, whereas  symlinks only hold addresses. As a further consequence of this, where hard links were unaffected by changing filenames, all symlinks are broken on changed file name.

A symlink can be created by adding a flag to ln:
ln -s [original name] [link name]

symlinks are far more popular than hard links due to their relative versatility and safety.

NOTE: windows shortcuts are fairly analogous to linux symlinks. Think of them that way.

## 'file'
Identifies what kind of file is provided as argument. eg. 'ASCII text' or 'symbolic link to [original loc]'

*Challenge 12: pwn.college{cd2wx0RA2P3awaqy2IyrZchF6SL.dlTM1UDLwEDN1czW}*

### solving challenge 12
I didn't fully understand the quesstion they were asking beyond ok we have to use symlinking to *some* capacity, so I went from there, thought ok, /challenge/catflag is going to read out the contents of the not-the-flag file, right. And further, I tried reading out /flag directly but the system rebuffed me. My means of recourse was thus to symlink flag to not-the-flag, so when the program catflag would read out not-the-flag, the symlink would instead cause it to read out /flag. Hence, indirectly accomplishing what the system would not let me directly do. It worked!
 
