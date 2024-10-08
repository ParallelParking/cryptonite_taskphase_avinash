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


