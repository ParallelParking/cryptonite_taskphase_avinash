# Practicing Piping

Linux's initial, standard  channels of communication:

1. Standard input: Channel through which process takes input, like our commands
2. Standard output: channel through which processes output normal data, such as the flag
3. Standard error: channel through which processed output error details.

These are given shorthand names: stdin, stdout and stderr.

//I meant to read up on the treatise given in the reading section of module 6, but it redirects to web.archive.org, which at the time of doing this is down due to a DDoS attack.

https://x.com/brewster_kahle/status/1843761077798220253

Proof can be found here. Time of writing is 09/10/2024, 02:45 am.

NOTE TO SELF: If there is time, look through resources tomorrow

## Redirecting output

The '>' character can redirect stdout contents into a file. For example:

```bash
$echo hi > filename
$cat filename
hi
```

*Challenge 1: pwn.college{UGHtAACuGbuftR-fl9_cA3pUZiR.dRjN1QDLwEDN1czW}*

*Challenge 2: pwn.college{wBjFj1Dpo9QATVHLjcBIx6bdoPb.dVjN1QDLwEDN1czW}*

The issue with '>' is that it will repeatedly overwrite the contents of the file it is writing into, i.e. it doesn't retain information across various commands. To combat this, and to write multiple outputs into the same file, we use '>>', which appends into the file as opposed to overwriting it.

*Challenge 3: pwn.college{U7Ik6AYe-TPikIS4v5APXBk1nYn.ddDM5QDLwEDN1czW}*

## File descriptors

A file descriptor (FD) is a number that describes a communication channel in linux.

https://www.rozmichelle.com/pipes-forks-dups/

Unix associates everything as a file, so for example the stdout channel is just a file that manages the display of data on the shell. Simmilarly, reading data from the keyboard implicitly actually means to read data from the file representing the keyboard.

In this context, input and output data is text data that goes in and out of a process

By default, each of our default channels also has a default specific file descriptor, associated with an open file, and processes use these FDs to handle data.

stdin: FD 0
stdout: FD 1
stderr: FD 2

FDs are stored in the FD table, where each channel has no idea where the data is sent or read from, or where the descriptor comes from or goes to. The channels simply deal with FDs, never data sources themselves, and the process need only handle the FD, not the file itself, which is the kernel's responsibility.

If more FDs are necessary, the lowest unused FD number is used to assign to new FD.

Thus, we can use FDs to specifically redirect channels into files. For example:

```bash
$/challenge/run 2> errors.log
```

will write the stderr channel data into file errors.log

Further, we can redirect multiple FDs at the same time. For example:

```bash
$some_command > output.log 2> errors.log
```

This writes the stdout (implicitly 1>) to output.log and stderr to errors.log

*Challenge 4: pwn.college{M17pRRx2c_30gV2kdWyGQjM8q6E.ddjN1QDLwEDN1czW}*

## Redirecting input

done with the '<' character (reverse of redirecting output). Otherwise identical.

*Challenge 5: pwn.college{Mbhmfaw2iGHiRW_qp3gg37n_0a8.dBzN1QDLwEDN1czW}*

Commands run:

```bash
$ echo COLLEGE > PWN
$ /challenge/run < PWN
```

*Challenge 6: pwn.college{UaPGNwIPCEwOCfOXQFAMM2foCFB.dhTM4QDLwEDN1czW}*

Commands run:

```bash
$ /challenge/run > /tmp/data.txt
$ grep pwn.college /tmp/data.txt
```

## The pipe operator '|'

outputs from command left of pipe will be "piped" into command right of pipe. This cuts out the middleman use of a file.

*Challenge 7: pwn.college{8aY_wnJHaNDxLsPJzaXMhIecQDF.dlTM4QDLwEDN1czW}*

Command run:

```bash
$ /challenge/run | grep pwn.college
```

## Grepping errors directly

This seems impossible, as there is no equivalent pipe operator for stderr, however we can redirect an FD to another FD.

We are simply routing the contents of some channel through another channel, for example as in this case, we plan on routing stderr through stdout. This makes our original problem straightorward, we can simply reroute stderr and then pipe grep it.

To accomplish this reroute we use the '>&' operator. For example: 2>& 1 reroutes FD2 through FD1 (as discussed, FD2 is stderr and FD1 is stdout)

*Challenge 8: pwn.college{Q6yu6Aiy1ywGhj25wxl4tX0d8G7.dVDM5QDLwEDN1czW}*

Command run:

```bash
$ /challenge/run 2>& 1 | grep pwn.college
```

## 'tee'

when data is piped between commands, it no longer displays in the shell, to deal with this, the tee command is used (named after the T-splitter from plumbing pipes). This tee command will duplicate data any number of times necessary. For example:

```bash
$ echo hi | tee pwn college
hi
$ cat pwn
hi
$ cat college
hi
```

*Challenge 9: pwn.college{s65khS2JBbZ2s0ly6klBFikA-dj.dFjM5QDLwEDN1czW}*

### Solving challenge 9: duplicating piped data with tee

I was initially quite confused as to exactly what the challenge wanted from me, but on trying to tee & pipe directly into challenge file the hint message helped me understand what to do. From there, it was fairly straightforward. I used tee as a debugging middleman tool to obtain the secret key and proceeded to pipe pwn file with secret key arg to college file.

Commands run:

```bash
$ /challenge/pwn | tee pwnop | /challenge/college
$ cat pwnop
$ /challenge/pwn --secret s65khS2J | /challenge/college
```

NOTE: See Module 5 for some notes on process substitution

\>(command) : here, writing to another command will provide input for mentioned command

The process subtitution does this by first reading from stdin, hooking up its input to a temporary "named file", processing via command, then writing to stdout.

Writing to the named file of command will pipe data to the stdin of the command. This is typically done using commands that take output files as argument, eg. tee.

For example:

```bash
$ echo HACK | tee >(rev)
HACK
KCAH
```

here, several things are happening:

1. bash starts up rev command, hooking named pipe to rev's standard input.
2. bash starts up tee, hooking pipe to its stdin and replacing its first arg with /path/to/named/pipe.tee
3. bash uses echo to print HACK into tee's stdin
4. tee reads HACKm writes it to stdout, then writes it into named pipe
5. named pipe is connected to rev's stdin, so rev reads HACK, reverses it, and writes KCAH to stdout.

*Challenge 10: pwn.college{soGeCL-pbSLLJbklCOUXmSTAuNi.dBDO0UDLwEDN1czW}*

Command run:

```bash
/challenge/hack | tee >(/challenge/the) | tee >(/challenge/planet)
```

### Intuition behind Challenge 10

program hack outputs something, piped to file the, data of the piped to write to file planet. Finally, contents of planet outputted.

*Challenge 11: pwn.college{ci80ivMLwI0t76de4jXrxAEh80q.dFDNwYDLwEDN1czW}*

### Solving challenge 11: split-piping stderr and stdout

We were told in the trivia section of an earlier challenge that command > >(command) was equivalent to command | command. This is, in broad strokes, true. However, the key difference helps us solve this problem:

command > >(command) > >(command) is NOT equivalent to command | command | command. in the former case, command1 is redirecting into both command2 and command3, whereas in the latter case, command1 flows into command2 flows into command3.

Thus, in this situation we must utilise the former, that allows us to simultaneously redirect into 2 programs. Further, we want to write stderr input into one of the programs, this can simply be done by replacing one > with a 2>.

Hence, the final command to solve the problem is:

```bash
$ cd /challenge/
$ ./hack > >(./planet) 2> >(./the)
```

Module 6, fin.
