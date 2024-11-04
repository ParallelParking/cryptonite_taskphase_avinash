# Digesting Documentation

https://man7.org/linux/man-pages/
Above link is lookup software for the linux man pages
man pages are software documentation detailing how and where to use linux commands.

NOTE: in module 3/2, I repeatedly referred to -a and similar addons to commands as "flags". This was because of some terminology I read. I've since discovered this is wrong and -a and similars are simply arguments that control command functionality. Keep this in mind. Add a disclaimer when possible to module 3/2.

--------------------------------------------------------
## A random aside: what does it mean to 'SSH in'?

Mostly borne out of my own curiosity while using pwn.college, probably not the best time to be doing this considering all of this is due tomorrow, but vibes.

https://www.cloudflare.com/learning/access-management/what-is-ssh/

SSH: Secure Shell
It is a protocol that allows a user to securely send commands to a computer over an unsecured network.

Cryptography is used to authenticate and encrypt the connection between devices.

In this situation, I as the 'hacker' am SSHing into a computer to find the hidden flag.

-------------------------------------------------------- 

*Challenge 1: pwn.college{ksbo8HwhYeoq_wD9xlCihMBT-dl.dRjM5QDLwEDN1czW}*

*Challenge 2: pwn.college{wwmtCH5jnZoZGs9ocHCJkeFS8QB.dVjM5QDLwEDN1czW}*

### Challenge 2: Learning complex usage

In challenge 2, we're taught essentially one thing, that arguments can *also* take arguments. This is important because it enables the existence of commands like find, which need both a search criteria (say name of file) and further an argument to that criteria (specific name to search for).

Given this idea, it drops on us a custom command that prints a file given the right argument. On starting the challenge, I ls'd the root to spot the flag file, which I could not directly access.

To get around this, I called the custom command with the file to print as the flag file. This worked.

## 'man'

man stands for manual, and another command can be given as argument to it. This will give a full documentation of the given documentation.

hit 'q' to quit the man page for a command.

*Challenge 3: pwn.college{QmhY3Z4_Lbn0XsRnuXCe28XOtLI.dRTM4QDLwEDN1czW}*

Some notes on traversing the man page:
/string: highlights all occurrences of string starting at first.
?string: highlights all occurrences of string starting at end
n: on searching, move to next occurrence
N: on searching, move to previous occurrence

*Challenge 4: pwn.college{4ID6q7XX111quT1qYGUGJk5LAs2.dVTM4QDLwEDN1czW}*

### Solving challenge 4: searching manuals

reasoned that the correct argument would have flag in the documentation. did /flag, found correct arg, worked.

NOTE: man pages contain documentation for ALL commands, so man itself also, therefore man man is a valid command. 

*Challenge 5: pwn.college{QacFIF29ITmn13DwhaqqPY8hkdu.dZTM4QDLwEDN1czW}*

### Solving challenge 5: searching for manuals

Fun one! entering man man in the terminal was a mess, but I found the command to perform a global search for substring 

```shell
man -K challenge
man --global-apropos challenge
```

I reasoned that the contents of the scrambled man page for /challenge/challenge must contain the word challenge and thus this search would hit, and it did.

It provided the correct argument to give the command and the flag was mine.

## Help args

Sometimes programs don't have man pages, in which case they're generally given a help argument for documentation.

This argument may be any of the following:
--help
-h
-?
help
/?

*Challenge 6: pwn.college{YUKJmyDuF5XuEYu1GjVR6WqDI6s.ddjM4QDLwEDN1czW}*

### Solving challenge 6: helpful programs

```shell
cd /challenge
./challenge --help
./challenge -p
./challenge -g 516
```

used documentation inside help to figure that I needed an integer value obtained by giving argument -p, and finally the flag given by argument -g FLAG_INT

## help for builtins

Some commands, unlike the ones written about on the man pages, are built into the shell itself. These are called "builtins". They're invoked the same way, but the shell can handle them in its own process instead of launching another program.

The list of shell builtins can be obtained by running 'help'

To get help on a specific builtin, it can be passed as an argument to help

*Challenge 7: pwn.college{k3hJNbObQuAc5zIfbPI6NIWZvG2.dRTM5QDLwEDN1czW}*


